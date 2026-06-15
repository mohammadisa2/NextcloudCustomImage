---
title: "Issue 26: Path SSD berubah dari sda1 ke sdb1"
parent: Issues
nav_order: 3
lang: id
lang_ref: issue-26-ssd-path-change
---

## 26. Issue: Path SSD Berubah dari `sda1` ke `sdb1`

Penyebab:

```txt
Linux memberi nama /dev/sda, /dev/sdb, /dev/sdc berdasarkan urutan device terdeteksi.
Nama ini tidak permanen.
Setelah reboot/reconnect, SSD yang tadinya /dev/sda1 bisa menjadi /dev/sdb1.
```

Cek:

```bash
lsblk -f
ls -lah /dev/disk/by-uuid/
ls -lah /dev/disk/by-id/
```

Solusi permanen:

```txt
Gunakan UUID di /etc/fstab
Gunakan mount point stabil seperti /mnt/nextcloud-ssd
Jangan gunakan path /media/devmon/sda1... sebagai path utama Docker
```

Kalau sudah terlanjur pakai path lama di Compose, pilihan aman:

```txt
1. Stop container
2. Mount SSD via UUID ke path yang Docker sudah pakai
3. Jadikan mount itu permanen di /etc/fstab
4. Start container lagi
5. Migrasi ke /mnt/nextcloud-ssd nanti saat maintenance
```

Contoh fstab:

```txt
UUID=<SSD_UUID> /media/devmon/sda1-ata-SSD-NAME ext4 defaults,nofail,x-systemd.device-timeout=10 0 2
```

Atau lebih rapi untuk setup baru:

```txt
UUID=<SSD_UUID> /mnt/nextcloud-ssd ext4 defaults,nofail,x-systemd.device-timeout=10 0 2
```
