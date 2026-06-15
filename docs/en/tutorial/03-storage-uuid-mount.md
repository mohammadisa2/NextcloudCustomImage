---
title: "2–4. Disk, UUID, Permanent Mount, and Data Folders"
parent: Tutorial (EN)
nav_order: 3
lang: en
lang_ref: tutorial-03-storage-uuid-mount
---

## 2. Check the Disk and Get the SSD/HDD UUID

Check the disks:

```bash
lsblk -f
```

Find the SSD/HDD partition you want to use, for example:

```txt
sda1 ext4 UUID=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

Save that UUID as:

```txt
<SSD_UUID>
```

Example:

```txt
<SSD_UUID> = 81e1d45c-3f32-4c29-9efa-626d4792c40a
```

---

## 3. Create a Permanent Mount via UUID

Create a stable mount point:

```bash
sudo mkdir -p <SSD_MOUNT>
```

Example:

```bash
sudo mkdir -p /mnt/nextcloud-ssd
```

Back up `/etc/fstab`:

```bash
sudo cp /etc/fstab /etc/fstab.bak.$(date +%Y%m%d-%H%M%S)
```

Add a permanent mount:

```bash
echo 'UUID=<SSD_UUID> <SSD_MOUNT> ext4 defaults,nofail,x-systemd.device-timeout=10 0 2' | sudo tee -a /etc/fstab
```

Example:

```bash
echo 'UUID=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx /mnt/nextcloud-ssd ext4 defaults,nofail,x-systemd.device-timeout=10 0 2' | sudo tee -a /etc/fstab
```

Reload systemd and mount:

```bash
sudo systemctl daemon-reload
sudo mount -a
```

Check:

```bash
findmnt <SSD_MOUNT>
```

Healthy output:

```txt
TARGET              SOURCE    FSTYPE OPTIONS
/mnt/nextcloud-ssd  /dev/sdX1 ext4   rw,relatime
```

Notes:

```txt
SOURCE may appear as /dev/sda1, /dev/sdb1, or /dev/sdc1.
What matters is that TARGET stays the same and OPTIONS contains rw.
```

---

## 4. Create Nextcloud Data Folders

Create the folders:

```bash
sudo mkdir -p <SSD_MOUNT>/docker/nextcloud/html
sudo mkdir -p <SSD_MOUNT>/docker/nextcloud/postgres
sudo mkdir -p <SSD_MOUNT>/docker/nextcloud/redis
```

Example:

```bash
sudo mkdir -p /mnt/nextcloud-ssd/docker/nextcloud/html
sudo mkdir -p /mnt/nextcloud-ssd/docker/nextcloud/postgres
sudo mkdir -p /mnt/nextcloud-ssd/docker/nextcloud/redis
```

Related issue references:

- [Issue: Server returns 403 and cannot read .htaccess](../issues/25-403-htaccess.md)
- [Issue: SSD path changes from sda1 to sdb1](../issues/26-ssd-path-changes.md)
- [Issue: EXT4 aborted journal and read-only filesystem](../issues/27-ext4-aborted-journal.md)
