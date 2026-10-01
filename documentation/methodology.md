# Methodology

## Scope

This lab is designed for authorized self-testing and cybersecurity education. Perform the exercise only against systems, browsers, devices, and accounts that you own or have explicit permission to test.

## Learning objectives

1. Understand how HTTP request metadata reaches a web server.
2. Identify common browser fingerprinting signals.
3. Understand browser geolocation permission.
4. Observe the effect of a reverse proxy/tunnel on the request path.
5. Practice source-code inspection and evidence collection.
6. Document security and privacy implications.

## Lab workflow

### Phase 1 — Prepare the environment

Use a Kali Linux VM or another isolated Linux lab environment.

Verify:

```bash
php --version
git --version
cloudflared --version
```

### Phase 2 — Obtain Hound

Use the upstream project rather than copying or redistributing its source:

https://github.com/techchipnet/hound

Clone it in your own authorized lab:

```bash
cd ~
git clone https://github.com/techchipnet/hound.git
cd hound
```

### Phase 3 — Inspect before execution

Read the documentation and inspect the relevant source files:

```bash
cat README.md
sed -n '1,240p' hound.sh
cat ip.php
cat script.js
cat webhook.php
cat template.php
```

The purpose of this phase is to understand what the application does before running it.

### Phase 4 — Local-only baseline

Start with a localhost test. The objective is to establish what information is observed when the browser and server are on the same machine.

Record:

- Request address
- User-Agent
- Browser/platform signals
- Application output

Do not publish raw collected data.

### Phase 5 — Tunnel concepts

Cloudflare Quick Tunnels can temporarily expose a local development service for testing. If you study tunneling, first practice with a harmless static page.

Example:

```bash
python3 -m http.server 8080 --bind 127.0.0.1
```

In a separate terminal:

```bash
cloudflared tunnel --url http://127.0.0.1:8080
```

Use the generated URL only for your own authorized test.

### Phase 6 — Geolocation

Browser geolocation is permission-controlled. Test only with your own browser/device.

Do not collect another person's precise location without informed authorization.

### Phase 7 — Evidence handling

Capture screenshots at meaningful stages. Before publishing:

- Remove precise coordinates.
- Remove personal/public IP addresses where appropriate.
- Remove cookies, tokens, credentials, and private URLs.
- Remove unrelated personal information.
- Keep raw evidence private.

## Evidence naming convention

```text
01-repository-setup.png
02-script-analysis.png
03-local-ip-collection-test.png
04-cloudflare-tunnel-success.png
05-hound-information-gathering-result.png
```

## Reproducibility

A learner should be able to reproduce the concepts without receiving anyone else's personal data. Use a fresh lab VM/browser profile and generate new evidence during each run.
