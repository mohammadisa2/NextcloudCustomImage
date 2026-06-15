---
title: "33. Update Strategy"
parent: Operations (EN)
nav_order: 4
lang: en
lang_ref: ops-04-update-strategy
---

## 33. Update Strategy

Do not auto-update carelessly.

Safe flow:

```txt
1. Run sudo nas-check
2. Confirm the system is healthy
3. Back up data and database
4. Update containers
5. Run sudo nas-check again
6. Check Nextcloud Overview
7. Check the logs
```

For Nextcloud:

```txt
Do not jump major versions without a backup.
Do not update when storage/mount is still unstable.
Do not update while active log errors are still present.
```

For manually deployed cloudflared:

```txt
Do not delete/rebuild it from CasaOS Legacy App.
Update it manually through Docker if needed.
```
