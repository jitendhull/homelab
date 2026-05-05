# Nextcloud

**Nextcloud** is a self-hosted file storage, synchronization, and sharing platform. Think of it as your personal Dropbox/Google Drive alternative.

## Overview

| Aspect | Details |
|--------|---------|
| **Purpose** | Cloud storage, file sync, calendars, contacts |
| **Image** | `nextcloud:latest` |
| **Port** | 8081 (configured in compose) |
| **Database** | MariaDB |
| **Cache** | Redis |
| **Storage** | `/home/ubuntu/nextcloud/` |

## Key Features

- File synchronization across devices
- Built-in calendar and contact management
- Collaboration tools
- Admin panel for user management
- Mobile apps available

## Prerequisites

- 2GB+ RAM (4GB+ recommended)
- 10GB+ storage
- Docker and Docker Compose installed

## Installation

### 1. Prepare Directories

```bash
# Create directories
sudo mkdir -p /home/ubuntu/nextcloud/{data,html,db,redis}

# Set permissions
sudo chown -R ubuntu:ubuntu /home/ubuntu/nextcloud
sudo chmod -R 755 /home/ubuntu/nextcloud
```

### 2. Deploy with Docker Compose

```bash
cd ~/homelab/cloud/nextcloud
docker compose up -d
```

This starts:
- **MariaDB** (database)
- **Redis** (caching)
- **Nextcloud** (main application)

### 3. Verify Deployment

```bash
# Check status
docker compose ps

# View logs
docker compose logs -f nextcloud

# Expected output: "Nextcloud is not installed yet"
```

### 4. Initial Configuration

Access Nextcloud at `http://localhost:8081` (or your domain if using reverse proxy).

1. Create admin account:
   - Username: `admin` (default)
   - Password: Create a strong password

2. Configure database connection:
   - **Database type**: MySQL
   - **Database user**: `nextcloud`
   - **Database password**: From `.env` file
   - **Database name**: `nextcloud`
   - **Database host**: `nextcloud-db` (internal hostname)

3. Complete setup by clicking "Install"

## Configuration

### Environment Variables

Edit in `docker-compose.yml` or use `.env`:

```bash
MYSQL_ROOT_PASSWORD=secure_root_pass
MYSQL_DATABASE=nextcloud
MYSQL_USER=nextcloud
MYSQL_PASSWORD=nextcloud_pass
NEXTCLOUD_ADMIN_USER=admin
NEXTCLOUD_ADMIN_PASSWORD=admin_password
NEXTCLOUD_TRUSTED_DOMAINS=localhost 127.0.0.1 nextcloud.yourdomain.com
```

### Important Settings

After initial setup, configure in Nextcloud admin panel:

**Admin Settings → Overview:**
- Trusted domains (add yours here)
- Database optimization
- Background jobs (set to "Cron")

```bash
# Configure cron job (runs background tasks)
* * * * * docker exec nextcloud sudo -u www-data php -f /var/www/html/cron.php
```

Add this to crontab:
```bash
crontab -e
```

### Enable Background Job via Cron

```bash
# SSH into container and set up cron
docker exec -it nextcloud bash
crontab -e

# Add line:
*/5 * * * * php -f /var/www/html/cron.php
```

## Usage

### Accessing Nextcloud

**Web Interface:**
```
http://localhost:8081
or
https://nextcloud.yourdomain.com (via reverse proxy)
```

**Sync Client (Desktop):**
1. Download from https://nextcloud.com/install/
2. Configure with your server URL
3. Log in with your credentials
4. Select folders to sync

**Mobile Apps:**
- Available on App Store and Play Store
- Search "Nextcloud"

### Creating Users

1. Log in as admin
2. Go to Administration → Users
3. Click "New User"
4. Set username, password, and groups

## Maintenance

### Backup

```bash
# Backup Nextcloud data
sudo tar -czf ~/backups/nextcloud-data.tar.gz /home/ubuntu/nextcloud/data

# Backup database
docker exec nextcloud-db mysqldump -u root -p$MYSQL_ROOT_PASSWORD nextcloud > ~/backups/nextcloud-db.sql
```

### Update

```bash
cd ~/homelab/cloud/nextcloud

# Pull latest image
docker compose pull

# Restart
docker compose up -d

# Check upgrade status
docker compose logs -f nextcloud
```

### Monitor Performance

```bash
# Check CPU/Memory usage
docker stats nextcloud

# View error logs
docker compose logs nextcloud | tail -50
```

## Troubleshooting

### "Cannot write to app folder"

```bash
# Fix permissions
docker exec -it nextcloud bash
chown -R www-data:www-data /var/www/html
chmod -R 755 /var/www/html
```

### Database Connection Error

```bash
# Verify database is running
docker compose ps

# Check database logs
docker compose logs nextcloud-db
```

### High CPU Usage

Enable Redis caching:
1. Admin Settings → System → Caching
2. Configure Redis settings
3. Restart containers

## Security Best Practices

- [ ] Use HTTPS (via reverse proxy)
- [ ] Enable two-factor authentication for admin
- [ ] Regular backups
- [ ] Keep Nextcloud updated
- [ ] Use strong passwords
- [ ] Enable password policy enforcement
- [ ] Configure firewall rules

## Storage Expansion

If you need more storage, mount additional drives:

```bash
# In docker-compose.yml, add volume:
volumes:
  - /mnt/large-storage/nextcloud:/var/www/html/data
```

Then restart:
```bash
docker compose up -d
```

## Advanced Configuration

### Mail Setup

Configure in Admin Settings → Mail:

```
From address: admin@yourdomain.com
Send mode: SMTP
Encryption: TLS
Authentication: Required
Server: your-mail-server
Port: 587
```

### Enable Full-Text Search

```bash
# Enable in Admin Settings
# Requires additional resources but improves search performance
```

## Next Steps

- 📱 Install sync clients on your devices
- 👥 Create user accounts
- 🔒 Set up SSL/TLS via reverse proxy
- 📊 Configure monitoring
- 💾 Set up automated backups

## Useful Commands

```bash
# Enter Nextcloud container
docker exec -it nextcloud bash

# Perform maintenance
docker exec -u www-data nextcloud php occ maintenance:mode --on
docker exec -u www-data nextcloud php occ db:convert-filecache-bigint
docker exec -u www-data nextcloud php occ maintenance:mode --off

# View recent errors
docker compose logs nextcloud | tail -100
```

## Resources

- [Official Nextcloud Documentation](https://docs.nextcloud.com/)
- [Docker Nextcloud Image](https://hub.docker.com/_/nextcloud)
- [Nextcloud Community](https://help.nextcloud.com/)

## See Also

- [Immich](immich.md) - Alternative photo management
- [Nginx Proxy Manager](../reverse-proxy/npm.md) - For HTTPS access
- [Backup & Recovery](../../admin/backup-recovery.md) - Data protection
