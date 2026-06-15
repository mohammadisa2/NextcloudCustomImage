---
title: "32. Backup Wajib"
parent: Operasional
nav_order: 3
---

## 32. Backup Wajib

Nextcloud bukan backup kalau hanya ada di satu SSD/HDD.

Minimal backup:

```txt
<SSD_MOUNT>/docker/nextcloud/html
<SSD_MOUNT>/docker/nextcloud/postgres
<SSD_MOUNT>/docker/nextcloud/redis
```

Yang paling penting:

```txt
Folder data user
Config Nextcloud
Database PostgreSQL
```

Tujuan backup ideal:

```txt
SSD/HDD lain
Komputer lain
Cloud storage
Remote server
Object storage seperti S3/R2
```

