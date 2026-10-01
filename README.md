# Hound Information-Gathering & Web Privacy Analysis Lab

![Platform](https://img.shields.io/badge/Platform-Kali%20Linux-557C94)
![Focus](https://img.shields.io/badge/Focus-Web%20Privacy%20%26%20Information%20Gathering-informational)
![Environment](https://img.shields.io/badge/Environment-Authorized%20Lab-green)

## Project Overview

This project documents an authorized cybersecurity laboratory exercise using **Hound**, a PHP-based information-gathering tool, to study how web applications can obtain network, browser, device, and permission-based geolocation information.

The experiment was performed against my own Kali Linux test environment and browser.

The objective was not to target or collect information from other people. The project focuses on understanding the technical data flow, privacy implications, and defensive controls associated with web-based information gathering.

## Objectives

- Deploy Hound in a controlled Kali Linux environment.
- Analyze the Hound source code and application architecture.
- Test server-side request information collection.
- Study browser/device fingerprinting signals.
- Understand browser-based geolocation permissions.
- Configure and test a Cloudflare Quick Tunnel in a controlled environment.
- Document the data flow from browser to PHP components.
- Identify privacy and security implications.
- Develop defensive recommendations.

## Lab Environment

| Component | Details |
|---|---|
| Operating System | Kali Linux |
| Web Server | PHP 8.4.x |
| Browser | Mozilla Firefox 140.x |
| Tunnel | Cloudflare Quick Tunnel |
| Application | Hound |
| Local Service | `127.0.0.1:8080` |
| Testing Scope | Own lab machine/browser |

## Architecture

```text
                    INTERNET
                       |
                       v
              Cloudflare Network
                       |
                Quick Tunnel URL
                       |
                       v
                 cloudflared
                       |
                       v
              +----------------+
              |   Kali Linux   |
              |                |
              | Hound / PHP    |
              |    :8080       |
              +-------+--------+
                      |
                      v
                Firefox Browser
                 (own test)
```

## Data Flow

```text
Browser
   |
   | HTTP/HTTPS request
   v
Cloudflare Tunnel
   |
   v
cloudflared
   |
   v
Hound / PHP
   |
   +----------------------+
   |                      |
   v                      v
Server-side data       Browser-side data
   |                      |
   v                      v
ip.txt                 data.txt
```

## Hound Components Analyzed

| File | Purpose observed during analysis |
|---|---|
| `hound.sh` | Main launcher, dependency checks, PHP/tunnel workflow |
| `index.php` | PHP entry point / request processing and redirection |
| `ip.php` | Processes request metadata and writes IP/User-Agent information |
| `script.js` | Browser-side interface and client-side functionality |
| `webhook.php` | Processes JSON input and appends data |
| `template.php` | Template/redirect functionality |
| `data.txt` | Application data output |
| `ip.txt` | IP/request information output |

## Test 1 — Local Request

The first test used the local PHP server:

```text
http://127.0.0.1:8080
```

The server recorded:

```text
IP: 127.0.0.1
User-Agent: Mozilla/5.0 ... Firefox/140.0
```

### Observation

Because the browser and PHP server were running on the same machine, the server observed the loopback address `127.0.0.1`.

This established the baseline before introducing a tunnel.

## Test 2 — Cloudflare Tunnel

A Cloudflare Quick Tunnel was used to expose the controlled development service temporarily.

The resulting architecture was:

```text
Firefox
   |
   v
trycloudflare.com
   |
   v
Cloudflare
   |
   v
cloudflared
   |
   v
127.0.0.1:8080
```

The tunnel successfully delivered the Hound application to my own Firefox browser.

## Test 3 — Information-Gathering Result

During the authorized self-test, Hound reported information including:

- IP address
- User-Agent
- Browser/platform information
- Browser language
- Cookie status
- Screen dimensions
- CPU-core information
- Operating-system information
- Local time
- Permission-based latitude/longitude

### Geolocation Result

The browser geolocation test successfully returned latitude and longitude after permission was granted.

**Public-repository privacy note:** the exact coordinates are intentionally redacted from this README. The complete values are retained only in the private lab evidence.

```text
Latitude:  [REDACTED]
Longitude: [REDACTED]
```

This demonstrates that browser geolocation is distinct from ordinary IP-based geolocation and depends on browser/device permission and available location services.

## Technical Analysis

### Server-side information

PHP can obtain request metadata through server variables such as:

```php
$_SERVER['REMOTE_ADDR']
$_SERVER['HTTP_USER_AGENT']
$_SERVER['HTTP_X_FORWARDED_FOR']
```

Applications must be careful when handling forwarding headers because proxy configuration determines which headers can be trusted.

### Browser information

Web applications can receive various browser-provided characteristics, including:

```text
User-Agent
Platform
Language
Screen dimensions
CPU-related browser information
Time-related information
```

Individually these signals may not uniquely identify a user, but combinations can contribute to browser fingerprinting.

### Geolocation

Browser geolocation follows a different model:

```text
Website
   |
   v
Browser Geolocation API
   |
   v
Permission request
   |
   +---- Denied ----> Coordinates unavailable
   |
   +---- Allowed ---> Coordinates returned
```

The experiment demonstrated the permission-controlled nature of browser geolocation.

## Security & Privacy Findings

### 1. Network metadata exposure

Web servers receive connection-related information when processing HTTP requests.

**Defensive consideration:** users should understand that visiting a web service necessarily exposes some network metadata to that service.

### 2. Browser fingerprinting

Multiple browser and device characteristics can be combined to create a more distinctive browser profile.

**Defensive consideration:** privacy-focused browser settings and anti-fingerprinting mechanisms can reduce exposed identifying characteristics.

### 3. Geolocation privacy

Precise browser geolocation can reveal significantly more information than a general IP-based location.

**Defensive consideration:** grant location permissions only to sites that genuinely need them and periodically review browser site permissions.

### 4. Reverse-proxy headers

Applications behind tunnels or reverse proxies may receive forwarding headers.

**Defensive consideration:** applications should explicitly define trusted proxies and avoid blindly trusting arbitrary client-supplied forwarding headers.

### 5. Public development tunnels

A temporary tunnel changes a service that was previously local into an Internet-reachable service.

**Defensive consideration:** use temporary tunnels only when required, avoid sensitive information in exposed development services, and shut down the tunnel after testing.

## Evidence

The project evidence was captured during the lab in chronological order:

```text
01-hound-repository-setup.png
02-hound-script-analysis.png
03-local-ip-collection-test.png
04-cloudflare-tunnel-success.png
05-hound-information-gathering-result.png
```

Additional analysis screenshots/diagrams can be placed in:

```text
evidence/
diagrams/
```

## Ethical Scope

This project was conducted in an authorized laboratory environment using my own machine and browser.

No third-party device, account, private network, or personal data belonging to another person was intentionally targeted.

The public GitHub version should not contain:

- Real third-party IP addresses
- Precise personal coordinates
- Session cookies
- Authentication tokens
- Private webhook URLs
- Other sensitive collected information

## Lessons Learned

1. A web server receives connection metadata as part of normal HTTP communication.
2. Browser characteristics can provide multiple fingerprinting signals.
3. Precise browser geolocation is permission-controlled.
4. Cloudflare Tunnel can expose a local development service without directly opening an inbound router port.
5. Reverse proxies change the network path and therefore require careful handling of forwarded headers.
6. Security testing should separate controlled experiments from public exposure.

## Defensive Recommendations

- Review browser location permissions regularly.
- Avoid granting unnecessary permissions to unfamiliar websites.
- Use privacy-conscious browser configurations.
- Treat development servers as potentially sensitive.
- Restrict public tunnels to controlled testing.
- Validate trusted reverse-proxy headers.
- Avoid storing sensitive information in publicly accessible logs.
- Redact sensitive evidence before publishing security projects.

## Skills Demonstrated

- Kali Linux
- Linux command-line administration
- Git/GitHub workflow
- PHP application analysis
- HTTP request analysis
- Browser fingerprinting concepts
- Web privacy analysis
- Cloudflare Tunnel
- Source-code inspection
- Security documentation
- Evidence collection
- Defensive security analysis

## Conclusion

This lab demonstrated how a web application can combine server-side request metadata with browser-provided information to build an information-gathering report.

The exercise also demonstrated why browser permissions, reverse-proxy configuration, public development tunnels, and careful handling of collected data are important components of web security and privacy.

The project was performed strictly as an authorized self-test and is intended for cybersecurity education and defensive analysis.
