---
title: "9–12. Cloudflare Domain + Tunnel"
parent: Tutorial (EN)
nav_order: 6
lang: en
lang_ref: tutorial-06-cloudflare-tunnel
---

## 9. Set Up the Domain in Cloudflare

Domain flow:

```txt
Buy a domain from a registrar
Add the domain to Cloudflare
Cloudflare gives you nameservers
Set those Cloudflare nameservers at your registrar
Wait until the domain becomes active in Cloudflare
```

Example domain:

```txt
<DOMAIN> = cloud.example.com
```

---

## 10. Set Up Cloudflare Tunnel

In Cloudflare:

```txt
Zero Trust
→ Networks
→ Tunnels
→ Create tunnel
```

Choose:

```txt
cloudflared
Docker
```

Cloudflare will provide a token.

Check the Docker network:

```bash
sudo docker network ls
```

The compose network is often named:

```txt
nextcloud_nextcloud-net
```

Run `cloudflared` manually:

```bash
sudo docker run -d \
  --name cloudflared \
  --restart unless-stopped \
  --network nextcloud_nextcloud-net \
  cloudflare/cloudflared:latest \
  tunnel --no-autoupdate run --token <CF_TUNNEL_TOKEN>
```

CasaOS note:

```txt
Because cloudflared is created manually through Docker CLI,
CasaOS may show it as a Legacy App / To be rebuilt.
That is normal.
Do not delete or rebuild cloudflared from CasaOS.
```

---

## 11. Set Up the Public Hostname in Cloudflare Tunnel

In the Cloudflare Tunnel panel:

```txt
Public Hostname
→ Add a public hostname
```

Fill in:

```txt
Hostname:
<DOMAIN>

Service type:
HTTP

Service URL:
nextcloud:80
```

Correct:

```txt
http://nextcloud:80
```

Incorrect:

```txt
http://localhost:8080
```

Reason:

```txt
cloudflared is on the same Docker network as Nextcloud.
So cloudflared must access Nextcloud directly through the container name nextcloud on port 80.
```

---

## 12. The Router Does Not Need Port Forwarding

Cloudflare Tunnel does not require port forwarding.

Recommended router state:

```txt
UPnP Off
DMZ Off
Port Trigger Off
Port Forwarding empty
```

Do not forward these ports to the internet:

```txt
80
443
8080
5432
6379
```

Local ports like these are still normal:

```txt
22   local SSH
80   local CasaOS gateway
8080 local Nextcloud access
```
