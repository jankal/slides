---
marp: true
theme: gaia
class: invert
paginate: true
_class: lead
_backgroundImage: linear-gradient( rgba(255, 255, 255, 0.1), rgba(255, 255, 255, 0.5) ), url('assets/shopware-ai-agent/slide-background.svg')
_color: white
---

<style>
  :root {
    --color-background: #1b4085;
    --color-foreground: #ddd;
    --color-highlight: #99b7d4;
    --color-dimmed: #888;
  }

  span.muted {
    color:rgb(111, 109, 109);
    font-size: 0.825rem;
  }

  pre {
    font-size: 0.7rem;
  }
</style>

# Shopware AI Agent

Autonomous Plugin Development with LLMs

<span class="muted">SCUC 2026 · @jankal · nuonic Digital</span>

---
<!-- backgroundColor: white -->
<!-- color: #71797E -->

# 🧠 Why?

- 🔁 Plugin development is **repetitive**
  - boilerplate: `composer.json`, Plugin class, `services.xml`...
  - same patterns over and over
- 🔀 Context switching is **costly**
  - docs, Admin API, CLI, code, browser
- 🤖 LLMs are great at code generation
  - but they lack **execution context**

---
<!-- backgroundColor: white -->
<!-- color: #71797E -->

# 🧠 The Gap

An LLM that can only *generate* code is a **fancy autocomplete**.

An LLM that can **write**, **execute**, **validate**, and **see** the result
is an **autonomous agent**.

→ We gave Claude a Shopware instance to play with.

---
<!-- _class: lead -->
<!-- _backgroundColor: rgb(55, 54, 54) -->
<!-- _color: rgb(227, 234, 239) -->

# 🐸 What?

---

# Architecture: Sidecar Pattern

```text
┌──────────── Kubernetes Pod ──────────────┐
│                                          │
│  ┌──────────────┐    ┌────────────────┐  │
│  │ shopware-app  │    │   ai-agent     │  │
│  │               │    │                │  │
│  │ PHP / Apache  │    │ Nuxt 4 / Bun   │  │
│  │ Shopware 6    │    │ Claude SDK     │  │
│  │ :80           │    │ :8000          │  │
│  └──────┬────────┘    └───────┬────────┘  │
│         │     Shared PVC      │           │
│         └──── /var/www/html ──┘           │
│                                          │
└──────────────────────────────────────────┘
        │                        │
   ┌────┴─────┐            ┌────┴─────┐
   │ MariaDB  │            │ Postgres │
   │ (Shop)   │            │ (Agent)  │
   └──────────┘            └──────────┘
```

---

# Why a Sidecar?

- Shared Volume = **zero-latency** file I/O
  - Agent writes PHP → instantly visible to Shopware
- `ReadWriteOnce` PVC is sufficient (same Pod)
  - no expensive RWX network storage needed
- **K8s Exec API** for command execution
  - no SSH, no network hops
- Independent lifecycle
  - Shopware crashes? Agent still runs.

---

# Tech Stack

| Component       | Technology                        |
|-----------------|-----------------------------------|
| Agent Runtime   | **Nuxt 4** + **Bun**              |
| LLM             | Claude Opus 4 (Anthropic SDK)     |
| Tool Bridge     | **MCP** (Model Context Protocol)  |
| K8s Integration | `@kubernetes/client-node`          |
| Conversations   | PostgreSQL + **Drizzle ORM**       |
| Validation      | `shopware-cli` (PHPStan, CS)      |
| Screenshots     | **Playwright** (Chromium)          |
| Frontend        | Nuxt UI + Tailwind CSS             |

---
<!-- _class: lead -->
<!-- _backgroundColor: rgb(55, 54, 54) -->
<!-- _color: rgb(227, 234, 239) -->

# ⚙️ How?

---

# Native Tools

The agent has **6 built-in tools** for direct Shopware interaction:

| Tool                 | What it does                           |
|----------------------|----------------------------------------|
| `write_file`         | Write to shared volume (with diff)     |
| `read_file`          | Read from shared volume                |
| `list_directory`     | Browse the filesystem                  |
| `search_files`       | Glob + content search                  |
| `exec_command`       | Shell commands via K8s Exec API        |
| `validate_extension` | PHPStan, code style via `shopware-cli` |

All paths relative to `/var/www/html` with **path traversal prevention**.

---

# K8s Exec API

```typescript
// k8s-exec.ts (simplified)
export async function execInContainer(command: string) {
  const kc = new k8s.KubeConfig()
  kc.loadFromCluster()

  const exec = new k8s.Exec(kc)

  await exec.exec(
    NAMESPACE,            // from Downward API
    POD_NAME,             // from Downward API
    'shopware-app',       // target container
    ['/bin/sh', '-c',
     `cd /var/www/html && su -s /bin/sh www-data -c '${command}'`],
    stdout, stderr,
    null,                 // no stdin
    false,                // no tty
    (status) => resolve(status)
  )
}
```

No SSH. No network hops. Just the **Kubernetes API**.

---

# MCP Server Tools

Three **MCP servers** extend the agent's capabilities:

**admin-mcp** (`@shopware-ag/admin-mcp`)
→ Products, categories, orders, themes, media via Admin API

**docs-mcp** (custom, Algolia)
→ Search developer.shopware.com documentation

**screenshot-mcp** (custom, Playwright)
→ Take screenshots, click elements, fill forms

<span class="muted">MCP = Model Context Protocol — open standard for LLM tool integration</span>

---

# MCP Bridge

```text
            ┌──────────────┐
            │  Claude LLM  │
            └──────┬───────┘
                   │ tool_use
            ┌──────┴───────┐
            │  MCP Bridge  │
            └──┬───┬───┬───┘
               │   │   │  StdioClientTransport
      ┌────────┘   │   └────────┐
      ▼            ▼            ▼
 admin-mcp    docs-mcp   screenshot-mcp
      │            │            │
      ▼            ▼            ▼
 Shopware     developer.    Playwright
 Admin API    shopware.com  Chromium
```

Tools are **discovered dynamically** — the bridge merges all MCP tools into the LLM tool palette at runtime.

---

# The Agentic Loop

```text
  User Prompt
      │
      ▼
┌──────────────────────────────┐
│  Claude generates response   │◄──────────┐
│  + tool_use blocks           │           │
└──────────┬───────────────────┘           │
           │ tool_use?                     │
     ┌─────┴─────┐                         │
     │ yes       │ no → done               │
     ▼                                     │
  Execute tools                            │
  (write, exec, screenshot...)             │
     │                                     │
     └── tool_results ─────────────────────┘
```

Up to **25 rounds** per conversation turn.
**SSE streaming** for real-time UI updates.

---

# Tool Tracking & Revert

Every `write_file` is tracked with full diffs:

```typescript
// dispatchTool (simplified)
case 'write_file': {
  const result = await writeFileOp(input.path, input.content)
  // result.diff → unified diff
  // result.previousContent → for revert

  await db.insert(toolActions).values({
    conversationId,
    toolName: 'write_file',
    diff: result.diff,
    previousContent: result.previousContent,
    isReversible: true,
  })
}
```

→ Users can **revert any file write** from the UI.

---

# System Prompt

The agent has a detailed **German system prompt** with:

- ✅ Shopware plugin directory structure
- ✅ Required `composer.json` fields
- ✅ CLI command reference (`plugin:refresh`, `cache:clear`, ...)
- ✅ Best practices (`strict_types`, PSR-4, namespaces)
- ✅ Admin API tool documentation
- ✅ Validation workflow (validate → fix → repeat)
- ✅ Screenshot workflow (compile → screenshot → show)

<span class="muted">~100 lines of domain knowledge baked into every request</span>

---
<!-- _class: lead -->

# 🎬 Demo

*"Create a plugin that adds a custom CMS element"*

---

# What Happens Behind the Scenes

1. 💬 User sends prompt via SSE stream
2. 🧠 Claude plans the plugin structure
3. 📝 `write_file` × 5–8 calls
   <span class="muted">composer.json, Plugin.php, services.xml, CmsElement, ...</span>
4. ⚡ `exec_command`: `plugin:refresh && plugin:install`
5. ⚡ `exec_command`: `plugin:activate && cache:clear`
6. ✅ `validate_extension`: PHPStan + code style
7. 🔧 Auto-fix any validation errors
8. 📸 `screenshot`: verify result in storefront
9. 💬 Claude reports back with screenshot

---

# Embedded in Shopware Admin

The agent UI runs as a **Shopware App** (not a plugin):

- `manifest.xml` with `<admin><module>`
- **Zero PHP code** — purely declarative
- iframe in Shopware Admin under Settings
- `postMessage('sw-app-loaded')` handshake
- Works even if Shopware is broken
  <span class="muted">(agent runs in a separate container)</span>

---

# Security

**Filesystem**
- Path traversal prevention (`safePath()`)
- All paths relative to `/var/www/html`

**Kubernetes**
- Namespace-scoped RBAC (ServiceAccount)
- Only `pods/exec` permission in own namespace
- Resource limits (500m CPU, 512Mi RAM)

**Agent**
- Max 25 tool rounds per turn
- 5-minute request timeout
- Full audit trail in PostgreSQL

---

# Lessons Learned

- 🎯 **Context is everything**
  Tools > more tokens. Let the LLM *do*, not just *say*.
- 🔄 **Validation loops are essential**
  LLMs make mistakes. Automated validation catches them.
- 📸 **Visual feedback closes the loop**
  Screenshots let the LLM self-correct UI issues.
- 🏗️ **MCP is the right abstraction**
  Swappable tool servers, standardized protocol.
- ⚡ **Sidecar > microservice**
  Shared volumes beat API calls for file I/O.

---

# What's Next?

- 🌐 Multi-tenant SaaS (scale-to-zero with KEDA)
- 🔐 Auth between Shopware ↔ Agent
- 📦 Git export to production repos
- 🧪 Automated test generation
- 🔌 More MCP servers (GitHub, CI/CD, monitoring)

---
<!-- _class: lead -->

# Questions?

<span class="muted">@jankal · nuonic Digital · SCUC 2026</span>
