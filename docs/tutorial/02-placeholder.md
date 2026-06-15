---
title: "1. Placeholder yang Harus Diganti"
parent: Tutorial
nav_order: 2
---

## 1. Placeholder yang Harus Diganti

Gunakan placeholder berikut selama tutorial:

```txt
<LAN_IP>          = IP lokal server, contoh 192.168.1.10
<DOMAIN>          = domain Nextcloud, contoh cloud.example.com
<SSD_UUID>        = UUID partisi SSD/HDD
<SSD_MOUNT>       = mount point permanen, contoh /mnt/nextcloud-ssd
<DB_PASSWORD>     = password database PostgreSQL yang kuat
<CF_TUNNEL_TOKEN> = token Cloudflare Tunnel dari dashboard Cloudflare
```

Contoh rekomendasi:

```txt
<SSD_MOUNT> = /mnt/nextcloud-ssd
```

Jangan jadikan path auto-mount seperti ini sebagai path utama Docker:

```txt
/media/devmon/sda1-...
/media/devmon/sdb1-...
```

Karena `sda1`, `sdb1`, `sdc1` bisa berubah setelah reboot atau storage reconnect.

Referensi issue terkait:

- [Issue: Path SSD berubah dari sda1 ke sdb1](../issues/26-path-ssd-berubah.md)

