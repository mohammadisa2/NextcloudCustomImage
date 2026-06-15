---
title: "1. Placeholders to Replace"
parent: Tutorial (EN)
nav_order: 2
lang: en
lang_ref: tutorial-02-placeholders
---

## 1. Placeholders to Replace

Use the following placeholders throughout the tutorial:

```txt
<LAN_IP>          = local server IP, example 192.168.1.10
<DOMAIN>          = Nextcloud domain, example cloud.example.com
<SSD_UUID>        = SSD/HDD partition UUID
<SSD_MOUNT>       = permanent mount point, example /mnt/nextcloud-ssd
<DB_PASSWORD>     = strong PostgreSQL database password
<CF_TUNNEL_TOKEN> = Cloudflare Tunnel token from the Cloudflare dashboard
```

Recommended example:

```txt
<SSD_MOUNT> = /mnt/nextcloud-ssd
```

Do not use auto-mounted paths like these as the main Docker path:

```txt
/media/devmon/sda1-...
/media/devmon/sdb1-...
```

Because `sda1`, `sdb1`, and `sdc1` can change after a reboot or a storage reconnect.

Related issue reference:

- [Issue: SSD path changes from sda1 to sdb1](../issues/26-ssd-path-changes.md)
