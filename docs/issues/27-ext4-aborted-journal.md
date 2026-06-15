---
title: "Issue 27: EXT4 aborted journal (filesystem read-only)"
parent: Issues
nav_order: 4
---

## 27. Issue: EXT4 Aborted Journal dan Filesystem Jadi Read-Only

Gejala di `dmesg`:

```txt
EXT4-fs error: ext4_journal_check_start
Detected aborted journal
Remounting filesystem read-only
```

Efek:

```txt
Nextcloud 403
occ tidak terbaca
nextcloud.log Input/output error
Database/Redis bisa error
```

Langkah aman:

```bash
sudo docker stop nextcloud nextcloud-postgres nextcloud-redis
```

Cek device:

```bash
lsblk -f
```

Unmount:

```bash
sudo umount <SSD_MOUNT>
```

Kalau busy:

```bash
sudo fuser -vm <SSD_MOUNT>
```

Jangan paksa dulu sebelum tahu proses mana yang menahan mount.

Repair filesystem:

```bash
sudo fsck.ext4 -f -y /dev/disk/by-uuid/<SSD_UUID>
```

Mount ulang:

```bash
sudo mount -a
findmnt <SSD_MOUNT>
```

Pastikan ada `rw`:

```bash
findmnt -no OPTIONS <SSD_MOUNT>
```

Start container:

```bash
sudo docker start nextcloud-postgres nextcloud-redis
sleep 5
sudo docker start nextcloud
```

Cek:

```bash
sudo docker exec -u www-data nextcloud php occ status
curl -I https://<DOMAIN>
```

Target:

```txt
maintenance: false
needsDbUpgrade: false
HTTP/2 302
location: https://<DOMAIN>/login
```

