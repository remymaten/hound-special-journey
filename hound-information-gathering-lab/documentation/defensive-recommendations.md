# Defensive Recommendations

## For end users

- Review browser location permissions.
- Deny location access when a site has no legitimate need for it.
- Avoid entering sensitive information into unfamiliar web applications.
- Keep browsers updated.
- Consider privacy-oriented browser protections.

## For developers

- Treat request metadata as untrusted input.
- Configure trusted reverse proxies explicitly.
- Validate forwarded headers.
- Avoid logging sensitive information unnecessarily.
- Protect log files from public exposure.
- Use HTTPS for production applications.
- Apply least privilege to application processes.
- Remove temporary development tunnels after testing.

## For security teams

Monitor for:

- Unexpected externally exposed development services.
- Unusual tunnel usage.
- Applications collecting excessive browser information.
- Sensitive data appearing in logs.
- Misconfigured reverse-proxy trust.

## Privacy principle

Collect only the information necessary for a legitimate purpose, explain that collection where appropriate, and protect the collected data throughout its lifecycle.
