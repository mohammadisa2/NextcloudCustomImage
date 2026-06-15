---
title: English
nav_order: 5
has_children: true
lang: en
lang_ref: home
---

# Nextcloud on CasaOS + Docker + PostgreSQL + Redis + Cloudflare Tunnel

Target:

- Home server / small NAS based on Armbian or Ubuntu + CasaOS
- Public Nextcloud over HTTPS without router port forwarding by using Cloudflare Tunnel
- Nextcloud data stored on SSD/HDD with a permanent UUID mount, not on root/eMMC

This documentation is generic. Replace placeholders such as `<DOMAIN>`, `<LAN_IP>`, `<SSD_UUID>`, `<SSD_MOUNT>`, `<DB_PASSWORD>`, and `<CF_TUNNEL_TOKEN>` according to your own server.

## How to read this documentation

- Follow the step-by-step guide in [Tutorial](tutorial/)
- Jump to [Issues](issues/) when you hit a specific problem
- Use [Operations](ops/) for log checks, `nas-check`, backups, and update strategy

## Final outcome (summary)

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
