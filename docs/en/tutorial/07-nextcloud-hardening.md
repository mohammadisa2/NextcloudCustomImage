---
title: "13–20. Post-Tunnel Configuration"
parent: Tutorial (EN)
nav_order: 7
lang: en
lang_ref: tutorial-07-hardening
---

## 13. Configure Nextcloud Trusted Domains

After the domain is active:

```bash
sudo docker exec -u www-data nextcloud php occ config:system:set trusted_domains 0 --value="<LAN_IP>:8080"
sudo docker exec -u www-data nextcloud php occ config:system:set trusted_domains 1 --value="<DOMAIN>"
```

Check:

```bash
sudo docker exec -u www-data nextcloud php occ config:system:get trusted_domains
```

Healthy output:

```txt
<LAN_IP>:8080
<DOMAIN>
```

---

## 14. Configure Trusted Proxy and HTTPS Overwrite

Check the Docker subnet:

```bash
sudo docker network inspect nextcloud_nextcloud-net | grep -A5 Subnet
```

Example subnet:

```txt
172.18.0.0/16
```

Set the trusted proxy:

```bash
sudo docker exec -u www-data nextcloud php occ config:system:set trusted_proxies 0 --value="172.18.0.0/16"
```

Set overwrite values:

```bash
sudo docker exec -u www-data nextcloud php occ config:system:set overwritehost --value="<DOMAIN>"
sudo docker exec -u www-data nextcloud php occ config:system:set overwriteprotocol --value="https"
sudo docker exec -u www-data nextcloud php occ config:system:set overwrite.cli.url --value="https://<DOMAIN>"
```

Check:

```bash
sudo docker exec -u www-data nextcloud php occ config:system:get trusted_proxies
sudo docker exec -u www-data nextcloud php occ config:system:get overwritehost
sudo docker exec -u www-data nextcloud php occ config:system:get overwriteprotocol
sudo docker exec -u www-data nextcloud php occ config:system:get overwrite.cli.url
```

---

## 15. Enable Redis in Nextcloud

```bash
sudo docker exec -u www-data nextcloud php occ config:system:set memcache.local --value='\OC\Memcache\APCu'
sudo docker exec -u www-data nextcloud php occ config:system:set memcache.locking --value='\OC\Memcache\Redis'
sudo docker exec -u www-data nextcloud php occ config:system:set memcache.distributed --value='\OC\Memcache\Redis'
sudo docker exec -u www-data nextcloud php occ config:system:set redis host --value="redis"
sudo docker exec -u www-data nextcloud php occ config:system:set redis port --value="6379" --type=integer
```

Check:

```bash
sudo docker exec -u www-data nextcloud php occ config:system:get memcache.locking
sudo docker exec -u www-data nextcloud php occ config:system:get redis
```

Healthy output:

```txt
\OC\Memcache\Redis

host: redis
port: 6379
```

Redis without a password is still acceptable if:

```txt
Redis is only reachable inside Docker
Port 6379 is not exposed to host/public
The router does not forward port 6379
```

---

## 16. Enable Cron Background Jobs

Set cron mode:

```bash
sudo docker exec -u www-data nextcloud php occ config:system:set backgroundjobs_mode --value="cron"
```

Edit the root crontab:

```bash
sudo crontab -e
```

Add:

```cron
*/5 * * * * /usr/bin/docker exec -u www-data nextcloud php -f /var/www/html/cron.php >/dev/null 2>&1
```

Check:

```bash
sudo crontab -l
sudo docker exec -u www-data nextcloud php occ config:system:get backgroundjobs_mode
sudo docker exec -u www-data nextcloud php occ config:app:get core lastcron
```

Healthy output:

```txt
cron
```

And `lastcron` should contain a numeric timestamp.

---

## 17. Set Region and Maintenance Window

Default phone region, example Indonesia:

```bash
sudo docker exec -u www-data nextcloud php occ config:system:set default_phone_region --value="ID"
```

The maintenance window uses UTC.

Example for 18 UTC:

```bash
sudo docker exec -u www-data nextcloud php occ config:system:set maintenance_window_start --type=integer --value=18
```

Check:

```bash
sudo docker exec -u www-data nextcloud php occ config:system:get default_phone_region
sudo docker exec -u www-data nextcloud php occ config:system:get maintenance_window_start
```

---

## 18. Add HSTS Header in Cloudflare

In Cloudflare:

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

Healthy output:

```txt
strict-transport-security: max-age=15552000; includeSubDomains
```

---

## 19. Hide the X-Powered-By Header

Check:

```bash
curl -I https://<DOMAIN> | grep -i powered
```

If it appears:

```txt
x-powered-by: PHP/x.x.x
```

Hide it through Cloudflare:

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

Important:

```txt
Do not choose Set static with value Remove.
That is wrong because it will produce X-Powered-By: Remove.
The action must be Remove.
```

Test:

```bash
curl -I https://<DOMAIN> | grep -i powered
```

If there is no output, it worked.

---

## 20. Test Public HTTPS

```bash
curl -I https://<DOMAIN>
```

Healthy output:

```txt
HTTP/2 302
location: https://<DOMAIN>/login
server: cloudflare
strict-transport-security: max-age=15552000; includeSubDomains
```

`HTTP/2 302` is normal because Nextcloud redirects users to the login page.
