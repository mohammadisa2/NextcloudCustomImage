---
title: "Issue 27: EXT4 aborted journal (read-only filesystem)"
parent: Issues (EN)
nav_order: 4
lang: en
lang_ref: issue-27-ext4-aborted-journal
---

## 27. Issue: EXT4 Aborted Journal and Filesystem Becomes Read-Only

Symptoms in `dmesg`:

```txt
EXT4-fs error: ext4_journal_check_start
Detected aborted journal
Remounting filesystem read-only
```

Effects:

```txt
Nextcloud 403
occ cannot be read
nextcloud.log Input/output error
Database/Redis may also fail
```

Safe steps:

```bash
sudo docker stop nextcloud nextcloud-postgres nextcloud-redis
```

Check the device:

```bash
lsblk -f
```

Unmount:

```bash
sudo umount <SSD_MOUNT>
```

If it is busy:

```bash
sudo fuser -vm <SSD_MOUNT>
```

Do not force it before you know which process is holding the mount.

Repair the filesystem:

```bash
sudo fsck.ext4 -f -y /dev/disk/by-uuid/<SSD_UUID>
```

Mount again:

```bash
sudo mount -a
findmnt <SSD_MOUNT>
```

Make sure it contains `rw`:

```bash
findmnt -no OPTIONS <SSD_MOUNT>
```

Start the containers:

```bash
sudo docker start nextcloud-postgres nextcloud-redis
sleep 5
sudo docker start nextcloud
```

Check:

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
