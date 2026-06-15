---
title: "5–6. Deploy with CasaOS + Check Containers"
parent: Tutorial (EN)
nav_order: 4
lang: en
lang_ref: tutorial-04-deploy-compose
---

## 5. Install Nextcloud via CasaOS Custom Install

Go to CasaOS:

```txt
CasaOS
→ App Store
→ Custom Install
```

Use the following Compose file.

Replace:

```txt
<SSD_MOUNT>
<DB_PASSWORD>
<LAN_IP>
```

```yaml
name: nextcloud

services:
  nextcloud:
    image: nextcloud:stable-apache
    container_name: nextcloud
    restart: unless-stopped
    ports:
      - 8080:80
    depends_on:
      - postgres
      - redis
    environment:
      POSTGRES_HOST: postgres
      POSTGRES_DB: nextcloud
      POSTGRES_USER: nextcloud
      POSTGRES_PASSWORD: <DB_PASSWORD>
      REDIS_HOST: redis
      PHP_MEMORY_LIMIT: 512M
      PHP_UPLOAD_LIMIT: 5G
      NEXTCLOUD_TRUSTED_DOMAINS: localhost <LAN_IP>
    volumes:
      - <SSD_MOUNT>/docker/nextcloud/html:/var/www/html
    networks:
      - nextcloud-net

  postgres:
    image: postgres:16-alpine
    container_name: nextcloud-postgres
    restart: unless-stopped
    command:
      - postgres
      - -c
      - shared_buffers=128MB
      - -c
      - max_connections=50
      - -c
      - work_mem=4MB
      - -c
      - maintenance_work_mem=64MB
    environment:
      POSTGRES_DB: nextcloud
      POSTGRES_USER: nextcloud
      POSTGRES_PASSWORD: <DB_PASSWORD>
    volumes:
      - <SSD_MOUNT>/docker/nextcloud/postgres:/var/lib/postgresql/data
    networks:
      nextcloud-net:
        aliases:
          - postgres

  redis:
    image: redis:alpine
    container_name: nextcloud-redis
    restart: unless-stopped
    command:
      - redis-server
      - --appendonly
      - "yes"
      - --maxmemory
      - 128mb
      - --maxmemory-policy
      - noeviction
    volumes:
      - <SSD_MOUNT>/docker/nextcloud/redis:/data
    networks:
      nextcloud-net:
        aliases:
          - redis

networks:
  nextcloud-net:
    driver: bridge
```

Important notes:

```txt
POSTGRES_HOST must be postgres
The database service name must be postgres
The network alias postgres must exist
```

If not, Nextcloud can fail with:

```txt
could not translate host name "postgres"
```

---

## 6. Check the Containers After Deployment

```bash
sudo docker ps
```

You should see:

```txt
nextcloud
nextcloud-postgres
nextcloud-redis
```

Check ports:

```bash
sudo ss -tulpn | grep -E '(:8080|:5432|:6379)'
```

Correct expectations:

```txt
8080 may listen on the host for local access
5432 PostgreSQL should not be exposed to host/public
6379 Redis should not be exposed to host/public
```

Related issue reference:

- [Issue: Initial install fails because postgres host/alias is wrong](../issues/08-initial-install-failure.md)
