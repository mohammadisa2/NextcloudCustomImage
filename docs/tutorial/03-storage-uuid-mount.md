---
title: "2–4. Disk, UUID, Permanent Mount, dan Folder Data"
parent: Tutorial
nav_order: 3
---

## 2. Cek Disk dan Ambil UUID SSD/HDD

Cek disk:

```bash
lsblk -f
```

Cari partisi SSD/HDD yang akan dipakai, misalnya:

```txt
sda1 ext4 UUID=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

Simpan UUID itu sebagai:

```txt
<SSD_UUID>
```

Contoh:

```txt
<SSD_UUID> = 81e1d45c-3f32-4c29-9efa-626d4792c40a
```

---

## 3. Buat Permanent Mount via UUID

Buat mount point stabil:

```bash
sudo mkdir -p <SSD_MOUNT>
```

Contoh:

```bash
sudo mkdir -p /mnt/nextcloud-ssd
```

Backup `/etc/fstab`:

```bash
sudo cp /etc/fstab /etc/fstab.bak.$(date +%Y%m%d-%H%M%S)
```

Tambahkan mount permanent:

```bash
echo 'UUID=<SSD_UUID> <SSD_MOUNT> ext4 defaults,nofail,x-systemd.device-timeout=10 0 2' | sudo tee -a /etc/fstab
```

Contoh:

```bash
echo 'UUID=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx /mnt/nextcloud-ssd ext4 defaults,nofail,x-systemd.device-timeout=10 0 2' | sudo tee -a /etc/fstab
```

Reload systemd dan mount:

```bash
sudo systemctl daemon-reload
sudo mount -a
```

Cek:

```bash
findmnt <SSD_MOUNT>
```

Output sehat:

```txt
TARGET              SOURCE    FSTYPE OPTIONS
/mnt/nextcloud-ssd  /dev/sdX1 ext4   rw,relatime
```

Catatan:

```txt
SOURCE boleh tampil sebagai /dev/sda1, /dev/sdb1, atau /dev/sdc1.
Yang penting TARGET tetap sama dan OPTIONS ada rw.
```

---

## 4. Buat Folder Data Nextcloud

Buat folder:

```bash
sudo mkdir -p <SSD_MOUNT>/docker/nextcloud/html
sudo mkdir -p <SSD_MOUNT>/docker/nextcloud/postgres
sudo mkdir -p <SSD_MOUNT>/docker/nextcloud/redis
```

Contoh:

```bash
sudo mkdir -p /mnt/nextcloud-ssd/docker/nextcloud/html
sudo mkdir -p /mnt/nextcloud-ssd/docker/nextcloud/postgres
sudo mkdir -p /mnt/nextcloud-ssd/docker/nextcloud/redis
```

Referensi issue terkait:

- [Issue: Server menampilkan 403 dan .htaccess tidak bisa dibaca](../issues/25-403-htaccess.md)
- [Issue: Path SSD berubah dari sda1 ke sdb1](../issues/26-path-ssd-berubah.md)
- [Issue: EXT4 aborted journal dan filesystem jadi read-only](../issues/27-ext4-aborted-journal.md)

