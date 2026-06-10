---
theme: default
title: "GEO-Tools im Vergleich"
titleTemplate: "%s – GEO Masterclass"
info: |
  60-Minuten-Segment: Tool-Vergleich
  GEO Masterclass · Stand der Daten: Juni 2026
highlighter: shiki
# hash routing: works on static hosting under a sub-path without a
# site-root 404.html fallback (deep links / refresh on slide N)
routerMode: hash
drawings:
  persist: false
transition: fade
mdc: true
fonts:
  sans: Inter
  mono: JetBrains Mono
---

# GEO-Tools im Vergleich

## Was euer Stack schon kann – und wann ihr mehr braucht

<div class="mt-12 text-sm opacity-70">
GEO Masterclass · Block: Tool-Landschaft · 60 Minuten
</div>

<!--
Begrüßung kurz halten – Kennenlernen lief bereits im gemeinsamen Teil.
Erwartungsmanagement: kein Feature-Feuerwerk, sondern ein Prüfschema.
-->

---

# Bevor wir starten: Disclosure

<div class="mt-8 text-xl leading-relaxed">

Ich baue mit <span class="mark">GEOlyze</span> selbst ein Tool in dieser Kategorie –
deshalb kenne ich diesen Markt von innen.

Im folgenden Vergleich lasse ich es <span class="mark">bewusst draußen</span>.

</div>

<div class="mt-10 text-sm opacity-60">
Wer dazu Fragen hat: gerne in der Diskussionsrunde.
</div>

<!--
Ein Satz, nicht mehr. Autorität + Transparenz, kein Pitch.
Danach sofort weiter – nicht auf der Folie verweilen.
-->

---

# Das Problem mit Tool-Listen

<div class="grid grid-cols-3 gap-8 mt-10">
<div>
<div class="kpi">20+</div>
<div class="kpi-label">GEO-Monitoring-Tools am Markt</div>
</div>
<div>
<div class="kpi">$300M+</div>
<div class="kpi-label">Funding allein 2025/26 in dieser Kategorie</div>
</div>
<div>
<div class="kpi">&lt; 2 J.</div>
<div class="kpi-label">Alter der meisten Plattformen</div>
</div>
</div>

<div class="mt-12 text-lg">
Die Tool-Liste von heute ist im Herbst veraltet.<br>
<span class="mark">Das Prüfschema nicht.</span>
</div>

<!--
Kernversprechen des Talks: Ihr geht nicht mit einer Einkaufsliste raus,
sondern mit einer Entscheidungslogik, die auch nächstes Jahr noch trägt.
-->

---

# Die Logik der nächsten 60 Minuten

<div class="mt-8 space-y-5 text-lg">

**0 · Einordnen** — SEO vs. GEO: was bleibt, was sich ändert <span class="text-sm opacity-60">(2 min Brücke)</span>

**1 · Messen** — Kommt AI-Traffic an? Und welche KPIs zählen jetzt? <span class="text-sm opacity-60">(GA4, kostenlos)</span>

**2 · Bestand prüfen** — Was können Sistrix, Ahrefs & Semrush inzwischen? <span class="text-sm opacity-60">(Herzstück)</span>

**3 · Methodik verstehen** — Welchem Score darf ich trauen? <span class="text-sm opacity-60">(Querschnitt)</span>

**4 · Eskalieren** — Wann lohnen Spezialtools? <span class="text-sm opacity-60">(Peec, Profound & Co.)</span>

**5 · Entscheiden** — Ein Entscheidungsbaum für euren Fall

</div>

<!--
Jeder Block beantwortet die Frage, die der vorherige aufwirft.
"Erst messen, dann tracken, dann optimieren" als Mantra wiederholen.
-->

---

# SEO vs. GEO – was die Tools verraten

<div class="grid grid-cols-2 gap-10 mt-6">
<div>

### Was bleibt (das Fundament)

- ChatGPT & Perplexity suchen über den <span class="mark">Bing-Index</span>, AI Overviews bauen auf Google-Rankings
- Wer nicht rankt, wird nicht gelesen – und nicht zitiert
- Ahrefs erklärt AI-Zitierungen über den **Link-Index**: Autorität wirkt weiter
- Technik, Struktur, Crawlability: unverändert Pflicht

</div>
<div>

### Was sich ändert (die Währung)

- **Antworten statt Rankings**: Es gibt keine Position 3 in einer KI-Antwort
- **Nennung statt Klick**: Sichtbarkeit wirkt, auch wenn niemand klickt
- **Entitäten statt Keywords**: Marken & Quellen, nicht Suchbegriffe
- Antworten sind **volatil**: zwei Anfragen, zwei Ergebnisse

</div>
</div>

<div class="mt-8 text-lg">
SEO bleibt die Eintrittskarte. <span class="mark">GEO ändert, womit gemessen wird.</span>
</div>

<!--
Brückenfolie – KEINE Grundlagen-Doppelung zum Co-Speaker-Part.
Falls die Kollegen das ausführlich behandeln: nur als 60-Sekunden-Recap
nutzen ("ihr habt vorhin gehört…") und schnell weiter.
Der Tool-Beweis ist der eigene Dreh: Die Suiten integrieren GEO,
WEIL die Datengrundlage dieselbe bleibt.
-->

---
layout: center
---

<div class="eyebrow">Block 1 · 10 min</div>

# Kommt überhaupt AI-Traffic?

<div class="mt-4 text-xl opacity-70">GA4 zeigt es euch – aber nicht freiwillig.</div>

---

# AI-Traffic versteckt sich in GA4

<div class="grid grid-cols-2 gap-10 mt-6">
<div>

### Was passiert

- Klicks aus ChatGPT, Perplexity & Co. landen in **Referral**, **Direct** oder **Unassigned**
- GA4 hat keinen Standard-Kanal für AI-Quellen
- Copy/Paste von URLs → kein Referrer → **„Direct"**

</div>
<div>

### Warum es zählt

- LLM-Traffic konvertiert deutlich besser als der Durchschnitt (Berichte: 30–40 %)
- AI-Referrals zu Top-Websites: **+357 % YoY**
- Wer nicht misst, kann GEO-Budget nicht begründen

</div>
</div>

<div class="src mt-8">Quellen: VentureBeat 04/2026 · TechCrunch 07/2025 · Stand: Juni 2026</div>

<!--
Live-Demo-Option: GA4 Explore öffnen, Session source/medium,
Filter auf "chatgpt|perplexity|gemini|claude|copilot".
Fallback: Screenshots, falls WLAN/Demo-Property zickt.
-->

---

# Die 5-Minuten-Lösung: Custom Channel Group

```text
Regex für Session source:
.*chatgpt.com.*|.*chat.openai.com.*|.*perplexity.*|
.*gemini.google.*|.*claude.ai.*|.*copilot.microsoft.*
```

<div class="grid grid-cols-2 gap-10 mt-6">
<div>

### Heute machbar

1. GA4 → Verwaltung → Channel-Gruppen
2. Neuen Kanal **„AI / LLM"** mit Regex anlegen
3. Rückwirkend in Explorationen nutzbar

</div>
<div>

### Ehrliche Grenze

**Dark AI Traffic** bleibt unsichtbar: Copy/Paste,
unterdrückte Referrer, Brand-Suche nach AI-Empfehlung.
→ Klick-Messung ist die <span class="mark">Untergrenze</span>, nie das Gesamtbild.

</div>
</div>

<!--
Take-away #1: Das setzt jeder am Montag um. Kostenlos, 5 Minuten.
Brücke zur KPI-Folie: "Klicks sind aber nur noch ein Ausschnitt –
was messen wir stattdessen?"
-->

---

# Vom Klick zur Nennung: das neue KPI-Set

| Ebene | KPI | Liefert |
|---|---|---|
| Präsenz | **Mention Rate** – Anteil Prompts mit Marken-Nennung | alle Suiten & Spezialisten |
| Wettbewerb | **Share of Voice** – eure Nennungen vs. Konkurrenz | Sistrix, Ahrefs, Peec, Profound |
| Quellen | **Citation Rate** – wird eure Domain als Quelle verlinkt? | Sistrix, Semrush, Profound |
| Qualität | **Sentiment** – wie wird über euch gesprochen? | Sistrix, Semrush, Spezialisten |
| Wirkung | **AI-Referral-Conversions** – nicht Sessions! | GA4 (Block 1) |

<div class="mt-6 text-lg">
Traffic ist nicht tot – aber er ist vom <span class="mark">Ziel zur Stichprobe</span> geworden:
~77 % der Suchen enden ohne Klick. Die Entscheidung fällt in der Antwort.
</div>

<div class="src mt-4">Click-Through-Daten: Branchenanalysen 2025/26 · Stand: Juni 2026</div>

<!--
Kernthese für die Stakeholder-Diskussion im Publikum:
Wer GEO weiter an Sessions misst, wird Budget verlieren, obwohl es wirkt.
AI-Referral-CONVERSIONS statt Sessions reporten – kleine Zahl, hohe Qualität
(30–40 % Conversion-Berichte aus Block 1 hier nochmal aufgreifen).
Diese Tabelle ist zugleich die Landkarte für Block 2: Jede Suite wird
gleich daran gemessen, welche dieser KPIs sie liefert.
-->

---
layout: center
---

<div class="eyebrow">Block 2 · 20 min</div>

# Was steckt schon in eurem Stack?

<div class="mt-4 text-xl opacity-70">Sistrix · Ahrefs · Semrush – gleiche vier Fragen an jedes Tool.</div>

<!--
Das Prüfschema ankündigen: Pro Suite immer dieselben 4 Fragen:
1. Was wird getrackt? 2. Wie werden Daten erhoben?
3. Was kostet es zusätzlich? 4. Wo ist die Grenze?
Die Wiederholung IST die Didaktik.
-->

---

# Sistrix · AI Prompt Tracker & Research

<div class="grid grid-cols-2 gap-8 mt-4">
<div>

### Was wird getrackt?

- **ChatGPT, Perplexity, Google AI Overviews** (Research-Tool zusätzlich: AI Mode)
- Täglicher AI-Sichtbarkeitsindex je Marke, Wettbewerber automatisch
- Sentiment je Nennung, Quellen-Analyse
- **Model Compare** & **Prompt Gap**: wo ist Konkurrenz sichtbar, ihr nicht?

### Wie erhoben?

<span class="mark">Crawling (simulierte Nutzer-Anfragen), keine APIs</span> → echte Antworten, wie Nutzer sie sehen

</div>
<div>

### Was kostet es?

**Kostenlos in der Beta** für alle Sistrix-Kunden.
Prompt-Kontingent richtet sich nach gebuchtem Paket.

### Wo ist die Grenze?

- Beta: Preis nach GA offen
- Engine-Abdeckung schmaler als bei Spezialisten (kein Claude, Grok, Meta AI)
- Export/API noch im Aufbau

</div>
</div>

<div class="src mt-4">Quelle: sistrix.de (FAQ, Handbuch, Changelog) · Stand: Juni 2026</div>

<!--
Für DACH-Publikum die Pflichtstation. Wer Sistrix hat, startet hier –
Argument: null Zusatzkosten, deutsche Datenbasis, vertraute Oberfläche.
Beta-Hinweis ehrlich bringen: Preismodell nach Beta-Ende ist offen.
-->

---

# Ahrefs · Brand Radar

<div class="grid grid-cols-2 gap-8 mt-4">
<div>

### Was wird getrackt?

- 6 AI-Plattform-Indexes (u. a. ChatGPT, Perplexity, AI Overviews, Copilot)
- **253 Mio.+ Prompts/Monat** aus echten Suchdaten – größte Prompt-DB der Kategorie
- Share of Voice, Trend, Wettbewerber
- Verknüpft mit 30-Billionen-Link-Index: *warum* zitiert AI bestimmte Domains?

### Wie erhoben?

Statische Prompt-Library + getimte Snapshots – <span class="mark">keine Live-Queries</span>

</div>
<div>

### Was kostet es?

Add-on zur Ahrefs-Basis (ab $129/M):
**$199/M pro AI-Index** · **$699/M alle 6**
→ realistisch **$328–828+/M**
2.500 Prompt-Checks inkl., danach $0,02/Check

### Wo ist die Grenze?

- Kein Claude, Grok, Meta AI
- Accuracy-Lücken (statische Library)
- Pro Domain – für Agenturen schnell vierstellig

</div>
</div>

<div class="src mt-4">Quellen: ahrefs.com · Peec-Vergleich 02/2026 · EWR Digital 02/2026 · Stand: Juni 2026</div>

<!--
Die Stärke ist Research, nicht Monitoring: die Prompt-DB für
Nachfrage-Analyse ("was fragen Leute wirklich?") ist einzigartig.
Das IST nebenbei der Prompt-Research-Teil dieses Talks.
Schwäche offen benennen: Preis + statische Library.
-->

---

# Semrush · AI Visibility Toolkit

<div class="grid grid-cols-2 gap-8 mt-4">
<div>

### Was wird getrackt?

- ChatGPT, Google AI Overviews, Gemini, Perplexity (Claude in Erweiterung)
- Prompt-Tracking mit täglichen Updates
- Anbindung an **Position Tracking, GA4, Search Console**
- AI-Readiness-Audit der eigenen Site

### Wie erhoben?

Eigene Erhebung; Methodik-Detail: <span class="mark">AI-Overview-Zitierung zählt als Position 1</span>

</div>
<div>

### Was kostet es?

**$99/Monat pro Domain** (Standalone, 1 User, ~25–50 Prompts je nach Plan)
Zusatz-Domain: +$99 · Zusatz-User: +$99
In **Semrush One**-Bundles enthalten (Starter $199: 50 Prompts · Pro+ $299: 100)

### Wo ist die Grenze?

- Kosten skalieren schnell (User × Domains)
- Score-Definitionen muss man kennen, bevor man ihnen traut

</div>
</div>

<div class="src mt-4">Quellen: semrush.com (KB, App Center) · Tekpon 04/2026 · Stand: Juni 2026</div>

<!--
"AI Overview = Platz 1" problematisieren: nachvollziehbare Vereinfachung,
aber sie verschiebt den Visibility-Score systematisch nach oben.
Wer reported, muss das den Stakeholdern erklären können.
Perfekte Überleitung zu Block 3 (Methodik).
-->

---

# Preisvergleich auf einen Blick

| | **Sistrix AI** | **Semrush AI Toolkit** | **Ahrefs Brand Radar** |
|---|---|---|---|
| Einstieg | <span class="mark">0 € (Beta)</span> zum Paket | $99/M pro Domain | $328/M (1 Index + Basis) |
| Vollausbau | Paket-Kontingent | $199–549/M (One-Bundle) | $828–1.148+/M |
| Engines | 3 (+AI Mode) | 4 (+Claude i. E.) | 6 |
| Erhebung | Crawling, täglich | eigene Erhebung | statische Library, Snapshots |
| Stärke | DACH-Daten, Sentiment, 0 € | GA4/GSC-Integration | Prompt-DB (253 M+), Link-Index |

<div class="mt-6 text-lg">
Spannweite: <span class="mark">Faktor 0 bis 1.000+ Dollar</span> – für nominell dieselbe Aufgabe.
</div>

<div class="src mt-4">Alle Preise: Herstellerangaben bzw. verifizierte Reviews · Stand: Juni 2026 · ändern sich derzeit monatlich</div>

<!--
Die Folie spricht für sich. Pointe: Der Preisunterschied erklärt sich
nicht durch Feature-Menge, sondern durch Datenerhebung und Zielgruppe.
Genau deshalb braucht ihr Block 3: Methodik.
-->

---
layout: center
---

<div class="eyebrow">Block 3 · 10 min</div>

# Welchem Score darf ich trauen?

<div class="mt-4 text-xl opacity-70">Drei Prüffragen, die jedes Tool beantworten muss.</div>

---

# Prüffrage 1: Crawling oder API?

<div class="grid grid-cols-2 gap-10 mt-6">
<div>

### Crawling (simulierte Nutzer)

- erfasst, was Nutzer **wirklich sehen**
- inkl. UI-Features, Personalisierungs-Effekte
- teurer, langsamer, fragiler
- *Beispiel: Sistrix*

</div>
<div>

### API-Abfragen

- schnell, günstig, skalierbar
- aber: API-Antworten ≠ Browser-Antworten
- kann systematisch abweichen
- *verbreitet bei günstigen Tools*

</div>
</div>

<div class="mt-10 text-lg">
Erste Frage an jeden Anbieter: <span class="mark">„Wie erhebt ihr eure Daten?"</span><br>
Wer das nicht in einem Satz erklären kann → Finger weg.
</div>

<!--
Das ist die wichtigste Einzelfrage der ganzen Evaluation.
ZipTie-Faustregel zitieren: API-Tracking als "echte Nutzerergebnisse"
zu verkaufen ist ein Red Flag.
-->

---

# Prüffrage 2 & 3: Prompts und Score-Definition

<div class="grid grid-cols-2 gap-10 mt-6">
<div>

### 2 · Wessen Prompts?

- **Statische Library** (Ahrefs): riesig, aber generisch & alternd
- **Eigene Prompt-Sets** (Sistrix, Semrush): relevant, aber Stichprobe
- Es gibt <span class="mark">kein „Suchvolumen für Prompts"</span> – alle Tools approximieren

</div>
<div>

### 3 · Was zählt der Score?

- Erwähnung ≠ Empfehlung: Nennung im *negativen* Vergleich zählt mit
- Semrush: AI Overview = Platz 1 – Annahme kennen!
- Score ohne erklärbare Methodik = <span class="mark">Zahl ohne Bedeutung</span>

</div>
</div>

<div class="mt-8 text-base opacity-70">
Pragmatischer Prompt-Research-Tipp: Sales-Gespräche, Support-Tickets und
People-Also-Ask schlagen jede generierte Liste.
</div>

<!--
Sentiment-Beispiel bringen: "Tool X ist veraltet, nimm lieber Y" –
das ist eine Erwähnung von X. Zählt die positiv? Nachfragen!
Prompt-Tipp ist der einzige Nicht-Tool-Moment des Talks – bewusst.
-->

---
layout: center
---

<div class="eyebrow">Block 4 · 10 min</div>

# Wann lohnen Spezialtools?

<div class="mt-4 text-xl opacity-70">Drei Eskalationsstufen – und ehrliche Wechselgründe.</div>

---

# Die Spezialisten als Eskalationsstufen

| | **Otterly** | **Peec AI** 🇩🇪 | **Profound** |
|---|---|---|---|
| Segment | Einstieg | Mid-Market | Enterprise |
| Preis | ab $29/M | ab €89/M | ab ~$499/M |
| Profil | simples Monitoring, 5 Engines | flexible Prompt-Sets, agenturtauglich | Antwort-Mechanik, Conversation Explorer, Fortune 500 |

<div class="mt-8">

### Wechseln lohnt sich, wenn …

- ihr **Multi-Client-Reporting** braucht (Agentur) – Suiten rechnen pro Domain ab
- Engines fehlen, die für euch zählen (Claude, Grok, Meta AI)
- ihr einen **Action-Layer** wollt: Empfehlungen statt nur Dashboards
- Monitoring-Daten in eigene **Workflows/APIs** fließen sollen

</div>

<div class="src mt-4">Stand: Juni 2026 · Markt konsolidiert sich – Preise vor Kauf prüfen</div>

<!--
Peec = Berlin, für DACH-Publikum relevant.
Gegenargument fairerweise nennen: "Messung ohne Aktion ist Theater" –
das teuerste Tool ist verschwendet, wenn niemand die Optimierung umsetzt.
-->

---

# Der Entscheidungsbaum

<div class="mt-6 text-lg leading-loose">

**Habt ihr Sistrix, Ahrefs oder Semrush?**
→ Ja: <span class="mark">Erst das Bordwerkzeug ausreizen.</span> Sistrix-Kunden: heute noch Beta aktivieren.

**Zeigt GA4 messbaren AI-Traffic?**
→ Nein: Channel Group einrichten, 3 Monate beobachten. Monitoring-Budget noch sparen.

**Agentur mit Kunden-Reporting oder fehlende Engines?**
→ Spezialist evaluieren: Otterly → Peec → Profound, in dieser Reihenfolge testen.

**In jedem Fall:**
→ Die drei Methodik-Fragen stellen – <span class="mark">vor</span> der Vertragsunterschrift.

</div>

<!--
Das ist die Folie, die fotografiert wird. Langsam durchgehen.
-->

---

# Drei Dinge für Montag

<div class="mt-10 space-y-6 text-xl">

**1.** GA4: Custom Channel Group **„AI / LLM"** anlegen – 5 Minuten, kostenlos

**2.** Vorhandene Suite prüfen: Sistrix-Beta aktivieren / Semrush-Toolkit testen / Ahrefs-Demo ansehen

**3.** Jedem Tool die Frage stellen: <span class="mark">„Wie genau erhebt ihr eure Daten?"</span>

</div>

<div class="mt-14 text-sm opacity-60">
Fragen? Jetzt – oder in der Diskussionsrunde am Ende.
</div>

<!--
Schluss unter Zeit anpeilen: Block 5 ist bewusst Puffer.
Bei Zeitnot: Entscheidungsbaum + diese Folie reichen als Abschluss.
-->