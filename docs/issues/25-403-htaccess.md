---
title: "Issue 25: 403 dan .htaccess tidak bisa dibaca"
parent: Issues
nav_order: 2
---

## 25. Issue: Server Menampilkan 403 dan `.htaccess` Tidak Bisa Dibaca

Gejala:

```txt
You don't have permission to access this resource.
Server unable to read htaccess file, denying access to be safe
HTTP/2 403
Could not open input file: occ
tail: cannot open nextcloud.log: Input/output error
```

Jangan langsung `chmod` atau `chown`.

Cek dulu mount SSD:

```bash
df -hT <SSD_MOUNT>
findmnt <SSD_MOUNT>
sudo docker exec -u www-data nextcloud php occ status
```

Kalau `occ` tidak bisa dibuka dan mount SSD hilang/read-only, berarti problemnya bukan permission, tapi storage/mount.

Rujukan issue terkait:

- [Issue 26: Path SSD berubah dari sda1 ke sdb1](26-path-ssd-berubah.md)
- [Issue 27: EXT4 aborted journal (filesystem read-only)](27-ext4-aborted-journal.md)

