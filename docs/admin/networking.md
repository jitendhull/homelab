# Networking

Networking setup and configuration guide.

## Local Network

### Static IP
Assign static IP to server:
```bash
sudo nano /etc/netplan/00-installer-config.yaml
sudo netplan apply
```

### Hostname
Set meaningful hostname:
```bash
sudo hostnamectl set-hostname homelab
```

### DNS
Configure DNS servers:
```bash
# Edit netplan
# nameservers:
#   addresses: [8.8.8.8, 8.8.4.4]
```

## Port Mapping

Only open:
- Port 80 (HTTP)
- Port 443 (HTTPS)
- Port 81 (NPM admin, optional)

Everything else stays internal.

## Domain Setup

### Point Domain to Server

1. Get your public IP: `curl ifconfig.me`
2. Log into domain registrar
3. Create A record: `yourdomain.com → your.public.ip`
4. Wait for DNS propagation (minutes to hours)

### Test DNS

```bash
nslookup yourdomain.com
# Should show your public IP
```

## Port Forwarding

Router setup:
1. Log into router admin (usually 192.168.1.1)
2. Find Port Forwarding section
3. Forward port 80 → server-ip:80
4. Forward port 443 → server-ip:443
5. Save and test

## Internal Network

Services communicate via Docker networks:
```
service1 ←→ service2 (internal, no firewall needed)
```

External access via reverse proxy:
```
internet ←→ nginx-proxy-manager ←→ service
```

## Firewall Rules

```bash
# View rules
sudo ufw status verbose

# Allow HTTP/HTTPS
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

# Allow SSH
sudo ufw allow 22/tcp
```

## Troubleshooting

### Cannot access service
- Check firewall: `sudo ufw status`
- Check port forwarding in router
- Verify DNS: `nslookup yourdomain.com`
- Check service running: `docker ps`

### Slow connection
- Check bandwidth: `iftop`
- Check network congestion
- Reduce video quality on Jellyfin
- Enable caching in services

See [Troubleshooting](../troubleshooting.md) for more.
