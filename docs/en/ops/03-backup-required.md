---
title: "32. Required Backup"
parent: Operations (EN)
nav_order: 3
lang: en
lang_ref: ops-03-backup-required
---

## 32. Required Backup

Nextcloud is not a backup if it exists only on one SSD/HDD.

Minimum backup:

```txt
<SSD_MOUNT>/docker/nextcloud/html
<SSD_MOUNT>/docker/nextcloud/postgres
<SSD_MOUNT>/docker/nextcloud/redis
```

Most important items:

```txt
User data folders
Nextcloud config
PostgreSQL database
```

Ideal backup destinations:

```txt
Another SSD/HDD
Another computer
Cloud storage
Remote server
Object storage such as S3/R2
```
