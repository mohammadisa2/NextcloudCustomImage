---
title: "21–24. Email, User/Quota, Upload, Best Practice"
parent: Tutorial (EN)
nav_order: 8
lang: en
lang_ref: tutorial-08-basic-operations
---

## 21. Set Up SMTP Email

In Nextcloud:

```txt
Admin settings
→ Basic settings
→ Email server
```

Gmail example:

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

If you see a log like:

```txt
User has no email address configured
```

Fill in the user's email at:

```txt
Personal settings
→ Personal info
→ Email
```

---

## 22. Users, Folders, and Quota

Nextcloud automatically creates:

```txt
1 user = 1 personal folder
```

Example:

```txt
admin
user1
user2
```

Actual folders on the server:

```txt
<SSD_MOUNT>/docker/nextcloud/html/data/admin/files
<SSD_MOUNT>/docker/nextcloud/html/data/user1/files
<SSD_MOUNT>/docker/nextcloud/html/data/user2/files
```

Set them from the UI:

```txt
Admin user
→ Users
→ New user
→ Set email
→ Set quota
```

Quota examples:

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

Line manager is not required for a personal NAS.

---

## 23. File Upload and Drag & Drop

Nextcloud web supports drag and drop:

```txt
Login to Nextcloud
→ Files
→ All files
→ drag file/folder into the middle area
```

Recommendation:

```txt
Small / medium uploads : Web browser
Large folder uploads   : Nextcloud Desktop Client
Automatic phone photos : Nextcloud iOS / Android app
```

---

## 24. Do Not Frequently Edit Files Directly Over SSH

User folder:

```txt
<SSD_MOUNT>/docker/nextcloud/html/data/<USER>/files
```

If you manually copy files through SSH, Nextcloud may not detect them immediately.

Scan a specific user:

```bash
sudo docker exec -u www-data nextcloud php occ files:scan <USER>
```

Scan all users:

```bash
sudo docker exec -u www-data nextcloud php occ files:scan --all
```
