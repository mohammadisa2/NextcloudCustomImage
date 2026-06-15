---
title: "Issue 08: Initial install failure"
parent: Issues (EN)
nav_order: 1
lang: en
lang_ref: issue-08-initial-install-failure
---

## 8. If the Initial Installation Fails

Common error:

```txt
could not translate host name "postgres"
```

Causes:

```txt
The database service is not named postgres
POSTGRES_HOST is wrong
The postgres network alias does not exist
Nextcloud and PostgreSQL containers are on different networks
```

Solution:

```txt
Make sure the compose file is correct
Make sure POSTGRES_HOST=postgres
Make sure the postgres service has the postgres network alias
Redeploy
```

If this is still a fresh install and there is no important data yet, you can remove the failed install config:

```bash
sudo rm -f <SSD_MOUNT>/docker/nextcloud/html/config/autoconfig.php
sudo rm -f <SSD_MOUNT>/docker/nextcloud/html/config/config.php
sudo docker restart nextcloud nextcloud-postgres nextcloud-redis
```

Warning:

```txt
Do not delete config.php if Nextcloud has already been used successfully and contains important data.
```

Related steps:

- [Deploy with CasaOS + Compose](../tutorial/04-deploy-compose.md)
- [Initial install from browser](../tutorial/05-initial-install.md)
