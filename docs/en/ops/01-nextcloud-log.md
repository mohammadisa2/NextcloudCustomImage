---
title: "29. Check and Clean Nextcloud Logs"
parent: Operations (EN)
nav_order: 1
lang: en
lang_ref: ops-01-log-nextcloud
---

## 29. Check and Clean Nextcloud Logs

Check UTC time:

```bash
date -u
```

Check the log:

```bash
sudo docker exec -u www-data nextcloud tail -n 20 /var/www/html/data/nextcloud.log
```

If the error timestamps are old and no longer appear after a newer check, it is acceptable to clear the log.

Back up and clear:

```bash
LOG="<SSD_MOUNT>/docker/nextcloud/html/data/nextcloud.log"

sudo cp "$LOG" "$LOG.bak.$(date +%Y%m%d-%H%M%S)"
sudo truncate -s 0 "$LOG"
```

Check:

```bash
sudo docker exec -u www-data nextcloud tail -n 20 /var/www/html/data/nextcloud.log
```

If it is empty, the log is clean.

Wait 1 to 2 minutes and check again:

```bash
sudo docker exec -u www-data nextcloud tail -n 20 /var/www/html/data/nextcloud.log
```

If it stays empty, the `Errors in the log` warning in Overview usually disappears after a refresh or after some time.
