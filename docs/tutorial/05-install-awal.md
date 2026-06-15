---
title: "7. Install Awal Nextcloud dari Browser"
parent: Tutorial
nav_order: 5
---

## 7. Install Awal Nextcloud dari Browser

Buka:

```txt
http://<LAN_IP>:8080
```

Pada halaman setup:

```txt
Buat akun admin
Pilih PostgreSQL
```

Isi database:

```txt
Database user     : nextcloud
Database password : <DB_PASSWORD>
Database name     : nextcloud
Database host     : postgres
```

Jangan gunakan SQLite untuk setup NAS jangka panjang.

Referensi issue terkait:

- [Issue: Install awal gagal (postgres host/alias salah)](../issues/08-install-awal-gagal.md)

