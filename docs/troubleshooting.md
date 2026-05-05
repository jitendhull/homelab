# Troubleshooting Guide

Common issues and solutions for homelab services.

## General Docker Issues

### "Cannot connect to Docker daemon"

```bash
# Start Docker
sudo systemctl start docker

# Enable on boot
sudo systemctl enable docker

# Verify
docker ps
```

### Permission Denied

```bash
# Add user to docker group
sudo usermod -aG docker $USER
newgrp docker

# Verify
docker run hello-world
```

### Container won't start

```bash
# Check logs
docker compose logs -f container_name

# Inspect container
docker inspect container_name

# Remove and recreate
docker compose down
docker compose up -d
```

## Service Issues

### Port Already in Use

```bash
# Find what's using the port
sudo lsof -i :80
sudo lsof -i :443
sudo lsof -i :8096

# Note the PID and kill it
sudo kill -9 <PID>

# Or change port in docker-compose.yml
```

### Out of Disk Space

```bash
# Check usage
df -h

# Find large files
sudo du -sh /home/ubuntu/*

# Clean up Docker
docker system prune -a
docker volume prune

# Remove logs
docker exec container_name sh -c 'truncate -s 0 /var/log/app.log'
```

### High Memory Usage

```bash
# Check memory
free -h

# Monitor containers
docker stats

# Reduce resource limits in docker-compose.yml:
# deploy:
#   resources:
#     limits:
#       memory: 512M
```

### Network Connectivity

```bash
# Test network
docker network ls
docker network inspect homelab-net

# Restart network
docker network disconnect container homelab-net
docker network connect homelab-net container
```

## Service-Specific Issues

### Nextcloud

**"Cannot write to app folder"**
```bash
docker exec nextcloud chown -R www-data:www-data /var/www/html
docker exec nextcloud chmod -R 755 /var/www/html
```

**Database connection failed**
```bash
# Check database
docker compose ps nextcloud-db
docker compose logs nextcloud-db

# Test connection
docker exec nextcloud-db mysql -u root -p$MYSQL_ROOT_PASSWORD -e "SELECT 1;"
```

### Jellyfin

**Cannot find media files**
```bash
# Verify path
ls -la /home/ubuntu/media

# Check permissions
sudo chmod -R 755 /home/ubuntu/media
sudo chown -R ubuntu:ubuntu /home/ubuntu/media

# Manually refresh in dashboard
```

**Transcoding errors**
```bash
# Check disk space
df -h

# Check logs
docker compose logs jellyfin | grep -i transcode
```

### Nginx Proxy Manager

**502 Bad Gateway**
```bash
# Check if service is running
docker compose ps

# Verify hostname/port in proxy config
# Test connectivity
docker exec npm_app curl http://service-name:port

# Check logs
docker compose logs app
```

**Certificate renewal failed**
```bash
# Force renewal
docker compose exec app certbot renew --force-renewal

# Restart
docker compose restart app

# Check cert status
docker compose exec app certbot certificates
```

## Performance Issues

### Slow Performance

```bash
# Check system resources
docker stats

# Review logs
docker compose logs -f

# Check disk I/O
iostat -x 1 5

# Optimize: Disable unnecessary features in service settings
```

### High CPU Usage

```bash
# Identify service
docker stats

# Check what's running
docker top container_name

# Reduce transcoding/processing
# Disable ML features if not needed
# Enable caching
```

## Database Issues

### Database Corruption

```bash
# Backup current state
docker exec container-db mysqldump -u user -ppass database > ~/backup.sql

# Repair
docker exec container-db mysqlcheck -u root -ppass --repair --all-databases

# Restart
docker compose restart
```

### Slow Database Queries

```bash
# Check status
docker exec container-db mysqlshow -u root -ppass --status

# Optimize tables
docker exec container-db mysqlcheck -u root -ppass -o -A

# Monitor
docker compose logs -f database
```

## Networking Issues

### Cannot access service externally

```bash
# Check firewall
sudo ufw status

# Open port
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

# Check port forwarding on router
# Verify DNS pointing to server
nslookup yourdomain.com

# Test connectivity
curl http://your-server-ip:port
```

### DNS not working

```bash
# Check DNS
cat /etc/resolv.conf

# Test resolution
nslookup google.com

# Restart DNS
sudo systemctl restart systemd-resolved

# Flush DNS cache
sudo systemctl restart systemd-resolved
```

## File Permission Issues

### Cannot access mounted volumes

```bash
# Check permissions
ls -la /home/ubuntu/service-name

# Fix ownership
sudo chown -R ubuntu:ubuntu /home/ubuntu/service-name

# Fix permissions
sudo chmod -R 755 /home/ubuntu/service-name

# For container:
docker exec container chown -R appuser:appgroup /path/in/container
```

## Backup Issues

### Backup failed

```bash
# Check disk space
df -h ~/backups

# Verify paths exist
ls -la /home/ubuntu/

# Run backup manually
docker exec db mysqldump -u user -ppass database | gzip > ~/backups/manual-backup.sql.gz
```

### Cannot restore

```bash
# Verify backup file
gunzip -t ~/backups/backup.sql.gz

# Try restore
gunzip < ~/backups/backup.sql.gz | docker exec -i container mysql -u user -ppass

# Check container logs after
docker compose logs -f
```

## Update Issues

### Service won't update

```bash
# Check current version
docker compose images

# Pull latest
docker compose pull

# Restart
docker compose down
docker compose up -d

# Check logs
docker compose logs -f
```

### Application breaks after update

```bash
# Rollback to previous version
docker compose down

# Edit docker-compose.yml: specify previous version tag
# image: nextcloud:28 (instead of latest)

docker compose up -d
```

## Getting Help

### Collect diagnostic information

```bash
# System info
uname -a
free -h
df -h

# Docker info
docker --version
docker compose --version
docker ps
docker images

# Service logs
docker compose logs > ~/logs.txt

# Share with support (remove passwords!)
```

### Check official documentation

- Nextcloud: https://docs.nextcloud.com/
- Jellyfin: https://jellyfin.org/docs/
- Docker: https://docs.docker.com/

### Community support

- Official forums and Discord servers
- GitHub issues for specific services
- Homelab communities on Reddit

## Prevention

- ✅ Regular backups
- ✅ Monitor resource usage
- ✅ Keep systems updated
- ✅ Document changes
- ✅ Test procedures before needing them
- ✅ Set up alerts for disk/memory
- ✅ Review logs regularly
