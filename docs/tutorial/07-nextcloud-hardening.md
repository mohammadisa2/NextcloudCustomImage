---
title: "13–20. Konfigurasi Setelah Tunnel"
parent: Tutorial
nav_order: 7
---

## 13. Konfigurasi Trusted Domain Nextcloud

Setelah domain aktif:

```bash
sudo docker exec -u www-data nextcloud php occ config:system:set trusted_domains 0 --value="<LAN_IP>:8080"
sudo docker exec -u www-data nextcloud php occ config:system:set trusted_domains 1 --value="<DOMAIN>"
```

Cek:

```bash
sudo docker exec -u www-data nextcloud php occ config:system:get trusted_domains
```

Output sehat:

```txt
<LAN_IP>:8080
<DOMAIN>
```

---

## 14. Konfigurasi Trusted Proxy dan Overwrite HTTPS

Cek subnet Docker network:

```bash
sudo docker network inspect nextcloud_nextcloud-net | grep -A5 Subnet
```

Contoh subnet:

```txt
172.18.0.0/16
```

Set trusted proxy:

```bash
sudo docker exec -u www-data nextcloud php occ config:system:set trusted_proxies 0 --value="172.18.0.0/16"
```

Set overwrite:

```bash
sudo docker exec -u www-data nextcloud php occ config:system:set overwritehost --value="<DOMAIN>"
sudo docker exec -u www-data nextcloud php occ config:system:set overwriteprotocol --value="https"
sudo docker exec -u www-data nextcloud php occ config:system:set overwrite.cli.url --value="https://<DOMAIN>"
```

Cek:

```bash
sudo docker exec -u www-data nextcloud php occ config:system:get trusted_proxies
sudo docker exec -u www-data nextcloud php occ config:system:get overwritehost
sudo docker exec -u www-data nextcloud php occ config:system:get overwriteprotocol
sudo docker exec -u www-data nextcloud php occ config:system:get overwrite.cli.url
```

---

## 15. Aktifkan Redis di Nextcloud

```bash
sudo docker exec -u www-data nextcloud php occ config:system:set memcache.local --value='\OC\Memcache\APCu'
sudo docker exec -u www-data nextcloud php occ config:system:set memcache.locking --value='\OC\Memcache\Redis'
sudo docker exec -u www-data nextcloud php occ config:system:set memcache.distributed --value='\OC\Memcache\Redis'
sudo docker exec -u www-data nextcloud php occ config:system:set redis host --value="redis"
sudo docker exec -u www-data nextcloud php occ config:system:set redis port --value="6379" --type=integer
```

Cek:

```bash
sudo docker exec -u www-data nextcloud php occ config:system:get memcache.locking
sudo docker exec -u www-data nextcloud php occ config:system:get redis
```

Output sehat:

```txt
\OC\Memcache\Redis

host: redis
port: 6379
```

Redis tanpa password masih aman jika:

```txt
Redis hanya internal Docker
Port 6379 tidak diexpose ke host/public
Router tidak forward port 6379
```

---

## 16. Aktifkan Cron Background Jobs

Set mode cron:

```bash
sudo docker exec -u www-data nextcloud php occ config:system:set backgroundjobs_mode --value="cron"
```

Edit crontab root:

```bash
sudo crontab -e
```

Tambahkan:

```cron
*/5 * * * * /usr/bin/docker exec -u www-data nextcloud php -f /var/www/html/cron.php >/dev/null 2>&1
```

Cek:

```bash
sudo crontab -l
sudo docker exec -u www-data nextcloud php occ config:system:get backgroundjobs_mode
sudo docker exec -u www-data nextcloud php occ config:app:get core lastcron
```

Output sehat:

```txt
cron
```

Dan `lastcron` berisi timestamp angka.

---

## 17. Set Region dan Maintenance Window

Default phone region, contoh Indonesia:

```bash
sudo docker exec -u www-data nextcloud php occ config:system:set default_phone_region --value="ID"
```

Maintenance window memakai UTC.

Contoh set jam 18 UTC:

```bash
sudo docker exec -u www-data nextcloud php occ config:system:set maintenance_window_start --type=integer --value=18
```

Cek:

```bash
sudo docker exec -u www-data nextcloud php occ config:system:get default_phone_region
sudo docker exec -u www-data nextcloud php occ config:system:get maintenance_window_start
```

---

## 18. Tambahkan HSTS Header di Cloudflare

Di Cloudflare:

```txt
Rules
→ Transform Rules
→ Response Header Transform Rules
→ Create rule
```

Rule:

```txt
Rule name:
Nextcloud HSTS

Condition:
Hostname equals <DOMAIN>

Action:
Set static header

Header name:
Strict-Transport-Security

Value:
max-age=15552000; includeSubDomains
```

Test:

```bash
curl -I https://<DOMAIN> | grep -i strict
```

Output sehat:

```txt
strict-transport-security: max-age=15552000; includeSubDomains
```

---

## 19. Sembunyikan Header X-Powered-By

Cek:

```bash
curl -I https://<DOMAIN> | grep -i powered
```

Kalau muncul:

```txt
x-powered-by: PHP/x.x.x
```

Sembunyikan via Cloudflare:

```txt
Rules
→ Transform Rules
→ Response Header Transform Rules
→ Create rule
```

Rule:

```txt
Rule name:
Remove X-Powered-By

Condition:
Hostname equals <DOMAIN>

Action:
Remove HTTP response header

Header name:
X-Powered-By
```

Penting:

```txt
Jangan pilih Set static dengan value Remove.
Itu salah karena hasilnya menjadi X-Powered-By: Remove.
Yang benar action-nya harus Remove.
```

Test:

```bash
curl -I https://<DOMAIN> | grep -i powered
```

Kalau tidak ada output, berarti berhasil.

---

## 20. Test HTTPS Publik

```bash
curl -I https://<DOMAIN>
```

Output sehat:

```txt
HTTP/2 302
location: https://<DOMAIN>/login
server: cloudflare
strict-transport-security: max-age=15552000; includeSubDomains
```

`HTTP/2 302` normal karena Nextcloud mengarahkan user ke halaman login.

