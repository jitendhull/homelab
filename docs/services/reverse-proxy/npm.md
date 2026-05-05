# Nginx Proxy Manager (NPM)

**Nginx Proxy Manager** is a web-based interface for managing Nginx reverse proxy with automatic SSL/TLS certificate management.

## Overview

| Aspect | Details |
|--------|---------|
| **Purpose** | Reverse proxy, SSL/TLS, domain routing |
| **Image** | `jc21/nginx-proxy-manager:latest` |
| **Admin Port** | 81 |
| **HTTP Port** | 80 |
| **HTTPS Port** | 443 |
| **Storage** | `./data`, `./letsencrypt` |

## Key Features

- Web-based admin interface
- SSL/TLS with Let's Encrypt
- Automatic certificate renewal
- Multiple domain support
- Proxy routing rules
- Basic auth and access lists

## Prerequisites

- Ports 80, 81, 443 open
- Domain(s) pointing to your server
- Docker and Docker Compose

## Installation

### 1. Deploy

```bash
cd ~/homelab/reverse-proxy/npm
docker compose up -d
```

### 2. Initial Access

Admin interface: `http://localhost:81`

Default credentials:
- Email: `admin@example.com`
- Password: `changeme`

**Change these immediately!**

### 3. Change Admin Password

1. Login to http://localhost:81
2. User menu → Change password
3. Set strong password

## Configuration

### Add a Domain

1. Dashboard → Proxy Hosts → Add Proxy Host
2. Configure:
   - **Domain names**: `nextcloud.yourdomain.com`
   - **Scheme**: `http` (internal)
   - **Forward hostname/IP**: `nextcloud` (Docker network)
   - **Forward port**: `80`
   - **Block Common Exploits**: ✓

### Enable SSL/TLS

After adding proxy host:

1. Click the domain → SSL tab
2. Click "Request New SSL Certificate"
3. Select Let's Encrypt
4. Agree to terms
5. Save

Certificate is automatically renewed before expiry.

## Adding Multiple Services

For each service, create a proxy host:

```
nextcloud.yourdomain.com  → nextcloud:80
jellyfin.yourdomain.com   → jellyfin:8096
immich.yourdomain.com     → immich_server:2283
vault.yourdomain.com      → vaultwarden:8080
```

## Security

### Enable Basic Auth

1. Add Access List
2. Proxy Host → Access tab
3. Select access list
4. Set username/password

### IP Whitelist

1. Proxy Host → Access tab
2. Add IP addresses to allow

## Accessing Services

### Before Setup
```
http://localhost:8081   (Nextcloud)
http://localhost:8096   (Jellyfin)
http://localhost:2283   (Immich)
```

### After Setup
```
https://nextcloud.yourdomain.com
https://jellyfin.yourdomain.com
https://immich.yourdomain.com
```

All traffic encrypted with SSL/TLS!

## Maintenance

### Update

```bash
cd ~/homelab/reverse-proxy/npm
docker compose pull
docker compose up -d
```

### Backup Certificates

```bash
sudo tar -czf ~/backups/npm-certs.tar.gz ./letsencrypt
```

### View Logs

```bash
docker compose logs -f app
```

## Troubleshooting

### "502 Bad Gateway"

- Service might be down: `docker ps`
- Check internal hostname/port
- Verify service running: `docker compose ps` (in service directory)

### Certificate Not Renewing

```bash
# Force renewal
docker compose exec app certbot renew --force-renewal
docker compose restart app
```

### DNS Resolution Issues

- Verify domain points to your server IP
- Wait for DNS propagation (can take hours)
- Test: `nslookup yourdomain.com`

## Tips

- Use DNS validation for wildcard certificates
- Set certificate renewal reminders
- Monitor certificate expiry dates
- Keep admin password secure
- Regular backups of config

## Common Proxy Configurations

### Nextcloud
```
Domain: nextcloud.yourdomain.com
Scheme: http
Hostname: nextcloud
Port: 80
Block exploits: ✓
```

### Jellyfin
```
Domain: jellyfin.yourdomain.com
Scheme: http
Hostname: jellyfin
Port: 8096
Block exploits: ✓
```

### Immich
```
Domain: immich.yourdomain.com
Scheme: http
Hostname: immich_server
Port: 2283
Block exploits: ✓
```

## Next Steps

- ✅ Deploy NPM
- 📝 Add all service proxy hosts
- 🔒 Enable SSL certificates
- 🔐 Configure access control
- 📊 Monitor certificate status

## See Also

- [Nextcloud - Trusted domains setup](../cloud/nextcloud.md)
- [Jellyfin - Remote access](../entertainment/jellyfin.md)
- [Security Best Practices](../../admin/security.md)
