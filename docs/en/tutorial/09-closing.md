---
title: "34–35. Final Checklist + Conclusion"
parent: Tutorial (EN)
nav_order: 9
lang: en
lang_ref: tutorial-09-closing
---

Full operations section:

- [Operations](../ops/)

---

## 34. Final Checklist

The setup can be considered complete if:

```txt
[ ] SSD/HDD is permanently mounted via UUID
[ ] Stable mount point exists, for example /mnt/nextcloud-ssd
[ ] Docker is running
[ ] CasaOS is running
[ ] Nextcloud container is Up
[ ] PostgreSQL container is Up
[ ] Redis container is Up
[ ] Nextcloud data is stored on SSD/HDD
[ ] Nextcloud is reachable locally at http://<LAN_IP>:8080
[ ] Domain is active in Cloudflare
[ ] Cloudflare Tunnel is active
[ ] cloudflared is attached to the Nextcloud Docker network
[ ] Public hostname points to http://nextcloud:80
[ ] Router does not forward ports to the server
[ ] trusted_domains contains local IP and domain
[ ] trusted_proxies contains the Docker subnet
[ ] overwritehost contains the domain
[ ] overwriteprotocol is https
[ ] overwrite.cli.url is https://domain
[ ] Redis cache is active
[ ] Cron is active
[ ] HSTS is active
[ ] X-Powered-By is hidden
[ ] SMTP email is configured if needed
[ ] Users and quotas are created
[ ] nas-check is available
[ ] Old error logs have been backed up and cleaned after the system was confirmed healthy
[ ] A backup strategy has been prepared
```

---

## 35. Conclusion

Best practice flow for setting up Nextcloud on CasaOS:

```txt
1. Prepare SSD/HDD with a permanent UUID mount.
2. Do not use changing sda/sdb paths as the main Docker path.
3. Install Nextcloud through custom Docker Compose.
4. Use PostgreSQL, not SQLite.
5. Use Redis for cache and file locking.
6. Store all Nextcloud data on SSD/HDD.
7. Use Cloudflare Tunnel so you do not need to open router ports.
8. Point the tunnel to http://nextcloud:80 inside the Docker network.
9. Set trusted domain, trusted proxy, and HTTPS overwrite values.
10. Enable cron.
11. Add HSTS.
12. Hide X-Powered-By.
13. Check health with nas-check.
14. Do not clear logs before confirming the error is no longer active.
15. Back up before updates and before storing important data.
```

If every checklist item is green, Nextcloud is ready for daily use for:

```txt
File storage
Desktop sync
Phone photo backup
Family / small-team users
Calendar
Contacts
Notes
Private file sharing
```
