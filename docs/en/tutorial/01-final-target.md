---
title: "0. Desired End State"
parent: Tutorial (EN)
nav_order: 1
lang: en
lang_ref: tutorial-01-final-target
---

## 0. Desired End State

Final architecture:

```txt
Internet
→ Cloudflare
→ Cloudflare Tunnel
→ cloudflared container
→ Nextcloud container
→ PostgreSQL container
→ Redis container
→ SSD/HDD permanent mount
```

Target setup:

```txt
Nextcloud is reachable from the internet via HTTPS
No router port forwarding required
Nextcloud data is stored on SSD/HDD, not on root/eMMC
Database uses PostgreSQL
Redis is enabled for cache and file locking
Cron background jobs are active
HSTS is enabled
The X-Powered-By header is hidden
Error logs are checked and cleaned only after the system is confirmed healthy
The SSD mount is made permanent via UUID so sda/sdb changes do not break the setup
```
