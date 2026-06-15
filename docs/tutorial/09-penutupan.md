---
title: "34–35. Checklist Final + Kesimpulan"
parent: Tutorial
nav_order: 9
lang: id
lang_ref: tutorial-09-closing
---

Halaman operasional lengkap:

- [Operasional](../ops/)

---

## 34. Checklist Final

Setup dianggap selesai kalau:

```txt
[ ] SSD/HDD mounted permanen via UUID
[ ] Mount point stabil, contoh /mnt/nextcloud-ssd
[ ] Docker aktif
[ ] CasaOS aktif
[ ] Nextcloud container Up
[ ] PostgreSQL container Up
[ ] Redis container Up
[ ] Data Nextcloud tersimpan di SSD/HDD
[ ] Nextcloud bisa dibuka lokal di http://<LAN_IP>:8080
[ ] Domain aktif di Cloudflare
[ ] Cloudflare Tunnel aktif
[ ] cloudflared masuk network Docker Nextcloud
[ ] Public hostname mengarah ke http://nextcloud:80
[ ] Router tidak port forward ke server
[ ] trusted_domains berisi IP lokal dan domain
[ ] trusted_proxies berisi subnet Docker
[ ] overwritehost berisi domain
[ ] overwriteprotocol berisi https
[ ] overwrite.cli.url berisi https://domain
[ ] Redis cache aktif
[ ] Cron aktif
[ ] HSTS aktif
[ ] X-Powered-By tersembunyi
[ ] Email SMTP sudah diset jika perlu
[ ] User dan quota sudah dibuat
[ ] nas-check tersedia
[ ] Log error lama sudah dibackup dan dibersihkan setelah aman
[ ] Backup strategy sudah disiapkan
```

---

## 35. Kesimpulan

Flow terbaik untuk setup Nextcloud di CasaOS:

```txt
1. Siapkan SSD/HDD dengan permanent mount via UUID.
2. Jangan pakai path sda/sdb yang bisa berubah sebagai path utama Docker.
3. Install Nextcloud via Docker Compose custom.
4. Gunakan PostgreSQL, bukan SQLite.
5. Gunakan Redis untuk cache dan file locking.
6. Simpan semua data Nextcloud di SSD/HDD.
7. Gunakan Cloudflare Tunnel agar tidak perlu buka port router.
8. Arahkan Tunnel ke http://nextcloud:80 di Docker network.
9. Set trusted domain, trusted proxy, dan overwrite HTTPS.
10. Aktifkan cron.
11. Tambahkan HSTS.
12. Sembunyikan X-Powered-By.
13. Cek status dengan nas-check.
14. Jangan clear log sebelum memastikan error tidak aktif.
15. Backup sebelum update dan sebelum menyimpan data penting.
```

Jika semua checklist sudah hijau, Nextcloud siap dipakai harian untuk:

```txt
File storage
Sync desktop
Backup foto HP
User keluarga/tim kecil
Calendar
Contacts
Notes
File sharing pribadi
```
