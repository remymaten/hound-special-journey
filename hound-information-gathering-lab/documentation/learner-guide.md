# Learner Guide

This repository is an educational companion to a Hound information-gathering lab.

## Recommended learning sequence

1. Read the main README.
2. Read `documentation/methodology.md`.
3. Understand the HTTP request lifecycle.
4. Review `ip.php` and identify the server variables it uses.
5. Review `script.js` and identify browser-side signals.
6. Run a localhost-only experiment against your own browser.
7. Study Cloudflare tunneling using the harmless static-page example.
8. Repeat the authorized self-test only if you understand the data being collected.
9. Compare the localhost and tunneled request paths.
10. Write your own findings and defensive recommendations.

## Questions to answer

- What information is available to a web server without special browser permissions?
- Which browser attributes can contribute to fingerprinting?
- Why is `127.0.0.1` different from a public IP?
- What changes when a reverse proxy is introduced?
- Why should forwarded headers be validated?
- Why does precise browser geolocation require permission?
- What should be removed before publishing security evidence?

## Challenge

Create a small table with:

| Data point | Source | Permission required? | Privacy impact |
|---|---|---|---|
| IP/request address | HTTP/PHP | No special browser permission | Medium |
| User-Agent | HTTP | No special browser permission | Low/Medium |
| Screen dimensions | Browser API | Usually no explicit location permission | Low/Medium |
| Language | Browser | Usually no explicit permission | Low |
| Precise location | Browser geolocation | Yes | High |

Then compare your own observations with the table.
