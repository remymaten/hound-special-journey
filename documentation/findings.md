# Findings

## Finding 01 — HTTP request metadata

A web server receives request metadata as part of normal HTTP communication. In the localhost baseline, the test server observed the loopback address `127.0.0.1` and the browser User-Agent.

### Security relevance

Applications should understand what metadata is naturally exposed by HTTP and avoid treating it as more trustworthy or precise than it is.

---

## Finding 02 — Browser fingerprinting signals

The application displayed several browser/device characteristics, including User-Agent, platform, language, screen dimensions, and CPU-related information.

### Security relevance

Individual signals may be common, but combinations can contribute to browser fingerprinting and recognition.

---

## Finding 03 — Browser geolocation

The authorized self-test returned latitude and longitude after the browser permitted location access.

### Security relevance

Precise location is sensitive information. Browser location permissions should be granted only when necessary and appropriate.

---

## Finding 04 — Reverse proxy/tunnel behavior

The tunnel introduced an intermediary between the browser and local PHP service.

### Security relevance

Applications behind proxies should understand trusted forwarding headers and should not blindly trust arbitrary client-supplied values.

---

## Finding 05 — Public exposure of development services

A tunnel can make a service that was previously reachable only from localhost accessible from the Internet.

### Security relevance

Temporary tunnels should be used only for controlled testing and should be shut down after the exercise.

---

## Defensive controls

- Review browser site permissions regularly.
- Limit location permissions.
- Use privacy-conscious browser settings where appropriate.
- Do not expose development services unnecessarily.
- Validate reverse-proxy headers.
- Protect logs containing sensitive information.
- Redact sensitive values before publishing evidence.
