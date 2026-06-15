---
title: "7. Initial Nextcloud Installation from Browser"
parent: Tutorial (EN)
nav_order: 5
lang: en
lang_ref: tutorial-05-initial-install
---

## 7. Initial Nextcloud Installation from Browser

Open:

```txt
http://<LAN_IP>:8080
```

On the setup page:

```txt
Create the admin account
Choose PostgreSQL
```

Fill in the database settings:

```txt
Database user     : nextcloud
Database password : <DB_PASSWORD>
Database name     : nextcloud
Database host     : postgres
```

Do not use SQLite for a long-term NAS deployment.

Related issue reference:

- [Issue: Initial install fails because postgres host/alias is wrong](../issues/08-initial-install-failure.md)
