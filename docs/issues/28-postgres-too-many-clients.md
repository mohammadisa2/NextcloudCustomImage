---
title: "Issue 28: PostgreSQL too many clients already"
parent: Issues
nav_order: 5
lang: id
lang_ref: issue-28-postgres-too-many-clients
---

## 28. Issue: PostgreSQL `too many clients already`

Gejala log Nextcloud:

```txt
Failed to connect to the database
FATAL: sorry, too many clients already
remaining connection slots are reserved for roles with the SUPERUSER attribute
```

Biasanya terjadi sesaat setelah restart ketika browser/app membuka banyak request bersamaan, misalnya Photos preview:

```txt
/apps/photos/api/v1/preview/...
```

Cek koneksi PostgreSQL:

```bash
sudo docker exec -it nextcloud-postgres psql -U nextcloud -d nextcloud -c "SHOW max_connections; SELECT count(*) AS current_connections FROM pg_stat_activity; SELECT state, count(*) FROM pg_stat_activity GROUP BY state ORDER BY count DESC;"
```

Output sehat untuk NAS kecil:

```txt
max_connections: 50
current_connections: kecil, contoh 6
active: kecil, contoh 1
```

Jika koneksi sudah turun, error itu biasanya hanya spike sementara.

Jangan langsung menaikkan `max_connections` di server RAM kecil. Menaikkan koneksi bisa menambah beban RAM.

Yang bisa dilakukan:

```txt
Tunggu beberapa menit setelah restart
Jangan langsung buka Photos dengan banyak preview setelah service baru naik
Cek ulang log setelah timestamp terbaru
Kalau error tidak muncul lagi, log boleh dibersihkan
```

Rujukan langkah terkait:

- [Cek dan bersihkan log Nextcloud](../ops/01-log-nextcloud.md)
