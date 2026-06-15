---
title: "5–6. Deploy via CasaOS + Cek Container"
parent: Tutorial
nav_order: 4
---

## 5. Install Nextcloud via CasaOS Custom Install

Masuk CasaOS:

```txt
CasaOS
→ App Store
→ Custom Install
```

Gunakan Compose berikut.

Ganti:

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

Catatan penting:

```txt
POSTGRES_HOST harus postgres
Nama service database harus postgres
Network alias postgres harus ada
```

Kalau salah, Nextcloud bisa error:

```txt
could not translate host name "postgres"
```

---

## 6. Cek Container Setelah Deploy

```bash
sudo docker ps
```

Harus ada:

```txt
nextcloud
nextcloud-postgres
nextcloud-redis
```

Cek port:

```bash
sudo ss -tulpn | grep -E '(:8080|:5432|:6379)'
```

Yang benar:

```txt
8080 boleh listen di host untuk akses lokal
5432 PostgreSQL tidak perlu expose ke host/public
6379 Redis tidak perlu expose ke host/public
```

Referensi issue terkait:

- [Issue: Install awal gagal (postgres host/alias salah)](../issues/08-install-awal-gagal.md)

