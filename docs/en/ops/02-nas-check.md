---
title: "30–31. nas-check (Periodic Audit)"
parent: Operations (EN)
nav_order: 2
lang: en
lang_ref: ops-02-nas-check
---

## 30. Periodic Audit Command `nas-check`

Create the command:

```bash
sudo tee /usr/local/bin/nas-check >/dev/null <<'EOF'
#!/usr/bin/env bash
set -e

DOMAIN="<DOMAIN>"
SSD="<SSD_MOUNT>"
NC_PATH="$SSD/docker/nextcloud"

echo "===== 1. SYSTEM ====="
date
uptime
free -h
df -hT /
df -hT "$SSD" || true

echo ""
echo "===== 2. DOCKER CONTAINERS ====="
docker ps --format "table {{.Names}}\t{{.Image}}\t{{.Status}}\t{{.Ports}}"

echo ""
echo "===== 3. NEXTCLOUD STATUS ====="
docker exec -u www-data nextcloud php occ status || echo "Nextcloud OCC failed"

echo ""
echo "===== 4. IMPORTANT NEXTCLOUD CONFIG ====="
echo "--- Trusted domains:"
docker exec -u www-data nextcloud php occ config:system:get trusted_domains || true
echo "--- Trusted proxies:"
docker exec -u www-data nextcloud php occ config:system:get trusted_proxies || true
echo "--- Overwrite:"
docker exec -u www-data nextcloud php occ config:system:get overwritehost || true
docker exec -u www-data nextcloud php occ config:system:get overwriteprotocol || true
docker exec -u www-data nextcloud php occ config:system:get overwrite.cli.url || true
echo "--- Cron:"
docker exec -u www-data nextcloud php occ config:system:get backgroundjobs_mode || true
docker exec -u www-data nextcloud php occ config:app:get core lastcron || true
echo "--- Redis:"
docker exec -u www-data nextcloud php occ config:system:get memcache.locking || true
docker exec -u www-data nextcloud php occ config:system:get redis || true

echo ""
echo "===== 5. STORAGE / SSD MOUNT ====="
docker inspect nextcloud --format '{{range .Mounts}}{{println .Source "->" .Destination}}{{end}}'
du -sh "$NC_PATH" 2>/dev/null || true

echo ""
echo "===== 6. CLOUDFLARED ====="
docker ps --filter "name=cloudflared" --format "table {{.Names}}\t{{.Image}}\t{{.Status}}"
docker logs cloudflared --since 10m 2>&1 | grep -E "ERR|error|Unable|failed|refused" || echo "No cloudflared error in last 10 minutes"

echo ""
echo "===== 7. HTTPS / HEADER CHECK ====="
curl -I "https://$DOMAIN" 2>/dev/null | grep -Ei "HTTP/|location:|strict-transport-security|x-powered-by|server:" || true

echo ""
echo "===== 8. PORTS EXPOSED ON HOST ====="
ss -tulpn | grep -E "(:22|:80|:443|:8080|:5432|:6379)" || true

echo ""
echo "===== 9. RECENT NEXTCLOUD LOG ====="
docker exec -u www-data nextcloud tail -n 20 /var/www/html/data/nextcloud.log || true

echo ""
echo "===== 10. DOCKER DISK USAGE ====="
docker system df

echo ""
echo "===== 11. RESOURCE USAGE ====="
docker stats --no-stream

echo ""
echo "===== DONE ====="
echo "Healthy checklist:"
echo "- Nextcloud installed true"
echo "- maintenance false"
echo "- needsDbUpgrade false"
echo "- cloudflared Up"
echo "- no recent cloudflared error"
echo "- HTTP/2 302 to /login"
echo "- HSTS present"
echo "- X-Powered-By hidden"
echo "- SSD detected"
echo "- Nextcloud data mounted to SSD"
echo "- cron active"
EOF

sudo chmod +x /usr/local/bin/nas-check
```

Edit placeholders:

```bash
sudo nano /usr/local/bin/nas-check
```

Replace:

```txt
DOMAIN="<DOMAIN>"
SSD="<SSD_MOUNT>"
```

Run:

```bash
sudo nas-check
```

Save the output:

```bash
sudo nas-check | tee ~/nas-check-$(date +%Y%m%d-%H%M%S).log
```

---

## 31. How to Read `nas-check` Output

Healthy:

```txt
Nextcloud Up
PostgreSQL Up
Redis Up
cloudflared Up
No cloudflared error in last 10 minutes
HTTP/2 302
location: https://<DOMAIN>/login
HSTS present
X-Powered-By does not appear
SSD mount detected
Cron active
Nextcloud log is empty or has no new error
```

Needs checking:

```txt
Root disk above 90%
Very small available RAM
maintenance: true
needsDbUpgrade: true
Many new cloudflared errors
HSTS missing
X-Powered-By appears again
SSD mount not detected
Filesystem is read-only
Nextcloud data is going to root/eMMC instead of the SSD
```
