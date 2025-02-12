---
marp: true
theme: gaia
class: invert
paginate: true
_class: lead
_backgroundImage: linear-gradient( rgba(255, 255, 255, 0.1), rgba(255, 255, 255, 0.5) ), url('assets/symfony-cli/slide-background.svg')
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
</style>

![bg right:30% 80%](assets/symfony-cli/symfony-cli-shopware.svg)

# Symfony CLI for Shopware development


---
<!-- backgroundColor: white -->
<!-- color: #71797E -->

# 🧠 Why?

- ⚡️ light-weight on
  - diskspace
  - performance penalty
  - filesystem performance
- ✅ official symfony way
  - Symfony Profiler integration
  - Platform.sh integration → Shopware PaaS 😉
- 🖥️ platform agnostic

---

# 🐸 What?

- single binary built with golang
- uses natively installed binaries for php and extensions
  (easily done with homebrew on mac 😉)
- global proxy service for local SSL
- env-injecting proxy commands

---
<!-- _class: lead -->
<!-- _backgroundColor: rgb(55, 54, 54) -->
<!-- _color: rgb(227, 234, 239) -->

# ⚙️ How?

---

# 🔗 Prerequisites

- Docker (e.g. using OrbStack on Mac)

- System PHP

<span class="muted">⚡️ quick install:</span>
```shell
brew install shivammathur/php/php@8.4
brew install shivammathur/extensions/xdebug@8.4
```

---
<!-- _class: lead -->

![bg right:33% drop-shadow](assets/symfony-cli/rocket-launch-67649_1920.jpg)

```shell
brew install symfony-cli/tap/symfony-cli
```

```shell
docker compose up -d
```

```shell
symfony serve
```

```shell
$ symfony serve
                                                                                                                        
 [WARNING] The local web server is optimized for local development and MUST never be used in a production setup.        
                                                                                                                        

                                                                                                                        
 [WARNING] Please note that the Symfony CLI only listens on 127.0.0.1 by default since version 5.10.3.                  
           You can use the --allow-all-ip or --listen-ip flags to change this behavior.                                 
                                                                                                                        

                                                                                                                        
 [OK] Web server listening                                                                                              
      The Web server is using PHP FPM 8.4.3                                                                             
      https://127.0.0.1:8078                          
                                                                                                                        

Stream the logs via symfony server:log
```

---

# Stream server logs

```shell
symfony server:log
```

---
<!-- _class: lead -->

```shell
brew services symfony-cli start
```

```shell
symfony local:server:ca:install
```

→ setup proxy!

![bg fit right](assets/symfony-cli/macos-configure-symfony-proxy.png)

---

# Configuration

Config file: `.symfony.local.yaml`

```yaml path=".symfony.local.yaml"
proxy:
    domains:
        - shop
http:
    daemon: true
    use_gzip: true
    port: 8078
```

To set up a different proxy TLD than `.wip`
```shell
symfony local:proxy:tld <tld>
```

---
### Custom PHP ini settings

Create a `.php.ini`-file in your project and set options
```ini
upload_max_filesize = 256M
memory_limit = 2G
```

### Different PHP version

Create a `.php-version` file with the required PHP version

---

# Running background tasks

```yaml path=".symfony.local.yaml"
...
workers:
    messenger_consume_async: ~
```

Just runs the `messenger:consume` command in the background (restarting when failed)

---
# Additional features

- Wildcard-Domains supported

```yaml path=".symfony.local.yaml"
proxy:
    domains:
        - *.shop
```
Configures `*.shop.wip` to be reachable - useful for multiple sales-channels


---
# Debugging

- running xDebug
  `.php.ini`
  ```ini
  xdebug.mode = debug
  ```
- Docker env variables
  ```shell
  symfony var:export --debug
  ```
---
# Tips and tricks

- fixating `docker-compose` services to local ports using a `compose.override.yaml`
- using command line HTTP tools with `https_proxy=$(symfony proxy:url) curl abc.wip/`
---

# Sources

- https://pixabay.com/photos/rocket-launch-rocket-space-shuttle-67649/
