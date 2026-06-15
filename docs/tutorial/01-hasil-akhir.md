---
title: "0. Hasil Akhir yang Diinginkan"
parent: Tutorial
nav_order: 1
lang: id
lang_ref: tutorial-01-final-target
---

## 0. Hasil Akhir yang Diinginkan

Arsitektur final:

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
Nextcloud bisa diakses dari internet via HTTPS
Tanpa membuka port router
Data Nextcloud tersimpan di SSD/HDD, bukan root/eMMC
Database menggunakan PostgreSQL
Redis aktif untuk cache dan file locking
Cron background jobs aktif
HSTS aktif
Header X-Powered-By disembunyikan
Log error dicek dan dibersihkan setelah aman
Mount SSD dibuat permanen via UUID agar tidak rusak saat sda/sdb berubah
```
