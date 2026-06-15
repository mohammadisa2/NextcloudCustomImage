---
title: "Issue 28: PostgreSQL too many clients already"
parent: Issues (EN)
nav_order: 5
lang: en
lang_ref: issue-28-postgres-too-many-clients
---

## 28. Issue: PostgreSQL `too many clients already`

Symptoms in the Nextcloud log:

```txt
Failed to connect to the database
FATAL: sorry, too many clients already
remaining connection slots are reserved for roles with the SUPERUSER attribute
```

This usually happens shortly after a restart when the browser/app opens many requests at the same time, for example Photos previews:

```txt
/apps/photos/api/v1/preview/...
```

Check PostgreSQL connections:

```bash
sudo docker exec -it nextcloud-postgres psql -U nextcloud -d nextcloud -c "SHOW max_connections; SELECT count(*) AS current_connections FROM pg_stat_activity; SELECT state, count(*) FROM pg_stat_activity GROUP BY state ORDER BY count DESC;"
```

Healthy output for a small NAS:

```txt
max_connections: 50
current_connections: low, for example 6
active: low, for example 1
```

If the connection count has already dropped again, the error was likely just a temporary spike.

Do not immediately increase `max_connections` on a low-RAM server. More connections can increase RAM usage.

What you can do:

```txt
Wait a few minutes after restart
Do not open Photos with many previews immediately after startup
Check the log again after a newer timestamp
If the error no longer appears, the log can be cleaned
```

Related step:

- [Check and clean Nextcloud logs](../ops/01-nextcloud-log.md)
