---
title: "9–12. Domain Cloudflare + Tunnel"
parent: Tutorial
nav_order: 6
lang: id
lang_ref: tutorial-06-cloudflare-tunnel
---

## 9. Setup Domain di Cloudflare

Flow domain:

```txt
Beli domain di registrar
Tambahkan domain ke Cloudflare
Cloudflare memberi nameserver
Pasang nameserver Cloudflare di registrar
Tunggu domain aktif di Cloudflare
```

Contoh domain:

```txt
<DOMAIN> = cloud.example.com
```

---

## 10. Setup Cloudflare Tunnel

Di Cloudflare:

```txt
Zero Trust
→ Networks
→ Tunnels
→ Create tunnel
```

Pilih:

```txt
cloudflared
Docker
```

Cloudflare akan memberi token.

Cek network Docker:

```bash
sudo docker network ls
```

Biasanya network compose bernama:

```txt
nextcloud_nextcloud-net
```

Jalankan cloudflared manual:

```bash
sudo docker run -d \
  --name cloudflared \
  --restart unless-stopped \
  --network nextcloud_nextcloud-net \
  cloudflare/cloudflared:latest \
  tunnel --no-autoupdate run --token <CF_TUNNEL_TOKEN>
```

Catatan CasaOS:

```txt
Karena cloudflared dibuat manual via Docker CLI,
di CasaOS bisa muncul sebagai Legacy App / To be rebuilt.
Itu normal.
Jangan delete atau rebuild cloudflared dari CasaOS.
```

---

## 11. Setup Public Hostname Cloudflare Tunnel

Di Tunnel Cloudflare:

```txt
Public Hostname
→ Add a public hostname
```

Isi:

```txt
Hostname:
<DOMAIN>

Service type:
HTTP

Service URL:
nextcloud:80
```

Yang benar:

```txt
http://nextcloud:80
```

Bukan:

```txt
http://localhost:8080
```

Alasannya:

```txt
cloudflared berada di Docker network yang sama dengan Nextcloud.
Jadi cloudflared harus akses Nextcloud langsung lewat nama container nextcloud port 80.
```

---

## 12. Router Tidak Perlu Port Forwarding

Cloudflare Tunnel tidak membutuhkan port forwarding.

Rekomendasi router:

```txt
UPnP Off
DMZ Off
Port Trigger Off
Port Forwarding kosong
```

Jangan forward port berikut ke internet:

```txt
80
443
8080
5432
6379
```

Port lokal seperti ini masih normal:

```txt
22   SSH lokal
80   CasaOS gateway lokal
8080 Nextcloud lokal
```
