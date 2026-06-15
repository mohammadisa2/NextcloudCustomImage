---
title: Indonesia
nav_order: 1
has_children: true
lang: id
lang_ref: home
---

# Nextcloud di CasaOS + Docker + PostgreSQL + Redis + Cloudflare Tunnel

Target:

- Server rumahan / NAS kecil berbasis Armbian atau Ubuntu + CasaOS
- Nextcloud publik via HTTPS tanpa port forwarding router (pakai Cloudflare Tunnel)
- Data Nextcloud tersimpan di SSD/HDD (mount permanen via UUID), bukan root/eMMC

Dokumentasi ini bersifat generic. Ganti semua placeholder seperti `<DOMAIN>`, `<LAN_IP>`, `<SSD_UUID>`, `<SSD_MOUNT>`, `<DB_PASSWORD>`, dan `<CF_TUNNEL_TOKEN>` sesuai server masing-masing.

## Cara baca dokumentasi

- Ikuti tutorial step-by-step di [Tutorial](tutorial/)
- Kalau ketemu masalah tertentu, langsung lompat ke [Issues](issues/)
- Untuk operasional harian (log, nas-check, backup, update), lihat [Operasional](ops/)

## Hasil akhir (ringkas)

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
