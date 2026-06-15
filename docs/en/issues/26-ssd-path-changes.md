---
title: "Issue 26: SSD path changes from sda1 to sdb1"
parent: Issues (EN)
nav_order: 3
lang: en
lang_ref: issue-26-ssd-path-change
---

## 26. Issue: SSD Path Changes from `sda1` to `sdb1`

Cause:

```txt
Linux assigns /dev/sda, /dev/sdb, /dev/sdc based on device detection order.
Those names are not permanent.
After reboot or reconnect, the SSD that used to be /dev/sda1 may become /dev/sdb1.
```

Check:

```bash
lsblk -f
ls -lah /dev/disk/by-uuid/
ls -lah /dev/disk/by-id/
```

Permanent solution:

```txt
Use UUID in /etc/fstab
Use a stable mount point such as /mnt/nextcloud-ssd
Do not use /media/devmon/sda1... as the main Docker path
```

If you already use the old path in Compose, a safer option is:

```txt
1. Stop the containers
2. Mount the SSD by UUID to the path Docker is already using
3. Make that mount permanent in /etc/fstab
4. Start the containers again
5. Migrate to /mnt/nextcloud-ssd later during maintenance
```

Example `fstab` entry:

```txt
UUID=<SSD_UUID> /media/devmon/sda1-ata-SSD-NAME ext4 defaults,nofail,x-systemd.device-timeout=10 0 2
```

Or a cleaner choice for a new setup:

```txt
UUID=<SSD_UUID> /mnt/nextcloud-ssd ext4 defaults,nofail,x-systemd.device-timeout=10 0 2
```
