---
title: "Issue 08: Install awal gagal"
parent: Issues
nav_order: 1
---

## 8. Jika Install Awal Gagal

Error umum:

```txt
could not translate host name "postgres"
```

Penyebab:

```txt
Service database bukan postgres
POSTGRES_HOST salah
Network alias postgres tidak ada
Container Nextcloud dan PostgreSQL beda network
```

Solusi:

```txt
Pastikan compose benar
Pastikan POSTGRES_HOST=postgres
Pastikan service postgres ada network alias postgres
Redeploy
```

Kalau masih fresh install dan belum ada data penting, boleh hapus config install gagal:

```bash
sudo rm -f <SSD_MOUNT>/docker/nextcloud/html/config/autoconfig.php
sudo rm -f <SSD_MOUNT>/docker/nextcloud/html/config/config.php
sudo docker restart nextcloud nextcloud-postgres nextcloud-redis
```

Peringatan:

```txt
Jangan hapus config.php kalau Nextcloud sudah berhasil dipakai dan sudah ada data penting.
```

Rujukan langkah terkait:

- [Deploy via CasaOS + Compose](../tutorial/04-deploy-compose.md)
- [Install awal dari browser](../tutorial/05-install-awal.md)

