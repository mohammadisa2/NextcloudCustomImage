---
title: "21–24. Email, User/Quota, Upload, Best Practice"
parent: Tutorial
nav_order: 8
---

## 21. Setup Email SMTP

Di Nextcloud:

```txt
Admin settings
→ Basic settings
→ Email server
```

Contoh Gmail:

```txt
Send mode      : SMTP
Encryption     : STARTTLS
From address   : username
Domain         : gmail.com
Host           : smtp.gmail.com
Port           : 587
Authentication : ON
Login          : username@gmail.com
Password       : Google App Password
```

Kalau ada log:

```txt
User has no email address configured
```

Isi email user di:

```txt
Personal settings
→ Personal info
→ Email
```

---

## 22. User, Folder, dan Quota

Nextcloud otomatis:

```txt
1 user = 1 folder pribadi
```

Contoh:

```txt
admin
user1
user2
```

Folder real di server:

```txt
<SSD_MOUNT>/docker/nextcloud/html/data/admin/files
<SSD_MOUNT>/docker/nextcloud/html/data/user1/files
<SSD_MOUNT>/docker/nextcloud/html/data/user2/files
```

Atur dari UI:

```txt
Admin user
→ Users
→ New user
→ Set email
→ Set quota
```

Quota bisa manual:

```txt
50 GB
100 GB
200 GB
500 GB
Unlimited
```

Via command:

```bash
sudo docker exec -u www-data nextcloud php occ user:setting user1 files quota "100 GB"
sudo docker exec -u www-data nextcloud php occ user:info user1
```

Line manager tidak wajib untuk NAS pribadi.

---

## 23. Upload File dan Drag & Drop

Nextcloud web mendukung drag & drop:

```txt
Login Nextcloud
→ Files
→ All files
→ drag file/folder ke area tengah
```

Rekomendasi:

```txt
Upload kecil / sedang  : Web browser
Upload folder besar    : Nextcloud Desktop Client
Foto HP otomatis       : Nextcloud iOS / Android app
```

---

## 24. Jangan Sering Edit File Langsung dari SSH

Folder user:

```txt
<SSD_MOUNT>/docker/nextcloud/html/data/<USER>/files
```

Kalau copy file manual lewat SSH, Nextcloud belum tentu langsung tahu.

Scan user tertentu:

```bash
sudo docker exec -u www-data nextcloud php occ files:scan <USER>
```

Scan semua user:

```bash
sudo docker exec -u www-data nextcloud php occ files:scan --all
```

