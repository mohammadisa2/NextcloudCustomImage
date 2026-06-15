---
title: "33. Update Strategy"
parent: Operasional
nav_order: 4
lang: id
lang_ref: ops-04-update-strategy
---

## 33. Update Strategy

Jangan auto-update sembarangan.

Flow aman:

```txt
1. Jalankan sudo nas-check
2. Pastikan kondisi sehat
3. Backup data dan database
4. Update container
5. Jalankan sudo nas-check lagi
6. Cek Overview Nextcloud
7. Cek log
```

Untuk Nextcloud:

```txt
Jangan lompat major version tanpa backup.
Jangan update saat storage/mount belum stabil.
Jangan update ketika log masih ada error aktif.
```

Untuk cloudflared manual:

```txt
Jangan delete/rebuild dari CasaOS Legacy App.
Update manual lewat Docker jika diperlukan.
```
