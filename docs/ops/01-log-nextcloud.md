---
title: "29. Cek dan Bersihkan Log Nextcloud"
parent: Operasional
nav_order: 1
---

## 29. Cek dan Bersihkan Log Nextcloud

Cek waktu UTC:

```bash
date -u
```

Cek log:

```bash
sudo docker exec -u www-data nextcloud tail -n 20 /var/www/html/data/nextcloud.log
```

Jika log error timestamp lama dan tidak muncul lagi setelah waktu pengecekan terbaru, boleh clear log.

Backup dan clear:

```bash
LOG="<SSD_MOUNT>/docker/nextcloud/html/data/nextcloud.log"

sudo cp "$LOG" "$LOG.bak.$(date +%Y%m%d-%H%M%S)"
sudo truncate -s 0 "$LOG"
```

Cek:

```bash
sudo docker exec -u www-data nextcloud tail -n 20 /var/www/html/data/nextcloud.log
```

Kalau kosong, log sudah bersih.

Tunggu 1–2 menit lalu cek lagi:

```bash
sudo docker exec -u www-data nextcloud tail -n 20 /var/www/html/data/nextcloud.log
```

Kalau tetap kosong, warning `Errors in the log` di Overview biasanya hilang setelah refresh / beberapa saat.

