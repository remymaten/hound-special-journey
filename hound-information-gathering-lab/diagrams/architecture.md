# Architecture Diagram

```text
                         INTERNET
                            |
                            v
                  +--------------------+
                  | Cloudflare Network |
                  +---------+----------+
                            |
                     Quick Tunnel
                            |
                            v
                    +---------------+
                    | cloudflared   |
                    +-------+-------+
                            |
                            v
                 +--------------------+
                 |    Kali Linux      |
                 |                    |
                 |   Hound / PHP      |
                 |     :8080          |
                 +---------+----------+
                           |
                           v
                    +-------------+
                    |   Firefox   |
                    |  Own test   |
                    +-------------+
```

## Data flow

```text
Browser
   |
   | HTTP/HTTPS
   v
Tunnel / Proxy
   |
   v
PHP application
   |
   +----------------------+
   |                      |
   v                      v
Request metadata       Browser data
   |                      |
   v                      v
ip.txt / logs          data.txt / report
```
