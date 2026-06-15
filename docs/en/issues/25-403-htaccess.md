---
title: "Issue 25: 403 and unreadable .htaccess"
parent: Issues (EN)
nav_order: 2
lang: en
lang_ref: issue-25-403-htaccess
---

## 25. Issue: Server Returns 403 and `.htaccess` Cannot Be Read

Symptoms:

```txt
You don't have permission to access this resource.
Server unable to read htaccess file, denying access to be safe
HTTP/2 403
Could not open input file: occ
tail: cannot open nextcloud.log: Input/output error
```

Do not immediately run `chmod` or `chown`.

Check the SSD mount first:

```bash
df -hT <SSD_MOUNT>
findmnt <SSD_MOUNT>
sudo docker exec -u www-data nextcloud php occ status
```

If `occ` cannot be opened and the SSD mount is missing or read-only, then the real issue is storage/mount, not permissions.

Related issues:

- [Issue 26: SSD path changes from sda1 to sdb1](26-ssd-path-changes.md)
- [Issue 27: EXT4 aborted journal and read-only filesystem](27-ext4-aborted-journal.md)
