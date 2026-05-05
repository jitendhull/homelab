# Backup & Recovery

Regular backups are essential for protecting your data and enabling disaster recovery.

## Backup Strategy

### 3-2-1 Rule

- **3 copies** of important data
- **2 different media** types
- **1 off-site** copy

## What to Backup

| Component | Frequency | Criticality |
|-----------|-----------|-------------|
| Service configs | Monthly | High |
| Database dumps | Weekly | High |
| Photo/Media library | Monthly | Medium |
| User documents | Weekly | High |

## Backup Methods

### Database Backups

#### Nextcloud (MariaDB)

```bash
# Full backup
docker exec nextcloud-db mysqldump -u root -p$MYSQL_ROOT_PASSWORD --all-databases > ~/backups/nextcloud-full.sql

# Single database
docker exec nextcloud-db mysqldump -u root -p$MYSQL_ROOT_PASSWORD nextcloud > ~/backups/nextcloud-db.sql

# Compressed
docker exec nextcloud-db mysqldump -u root -p$MYSQL_ROOT_PASSWORD nextcloud | gzip > ~/backups/nextcloud-db.sql.gz
```

#### Immich (PostgreSQL)

```bash
docker exec immich_postgres pg_dump -U postgres immich | gzip > ~/backups/immich-db.sql.gz
```

### File Backups

#### Using tar

```bash
# Nextcloud data
sudo tar -czf ~/backups/nextcloud-data.tar.gz /home/ubuntu/nextcloud/

# Jellyfin config
sudo tar -czf ~/backups/jellyfin-config.tar.gz /home/ubuntu/jellyfin/config

# All service data
sudo tar -czf ~/backups/services-all.tar.gz /home/ubuntu/
```

#### Using rsync (for incremental)

```bash
# Sync to external drive
rsync -av /home/ubuntu/ /mnt/backup/homelab-data/

# Sync to remote server
rsync -av -e ssh /home/ubuntu/ user@backup-server:/backups/homelab/
```

### Automated Backups

Create a backup script:

```bash
#!/bin/bash
# /usr/local/bin/homelab-backup.sh

BACKUP_DIR="/home/ubuntu/backups"
DATE=$(date +%Y%m%d-%H%M%S)

# Create backup directory
mkdir -p $BACKUP_DIR/$DATE

# Backup databases
docker exec nextcloud-db mysqldump -u root -p$MYSQL_ROOT_PASSWORD nextcloud | gzip > $BACKUP_DIR/$DATE/nextcloud-db.sql.gz
docker exec immich_postgres pg_dump -U postgres immich | gzip > $BACKUP_DIR/$DATE/immich-db.sql.gz

# Backup configs
tar -czf $BACKUP_DIR/$DATE/services-config.tar.gz \
  /home/ubuntu/nextcloud/config \
  /home/ubuntu/jellyfin/config \
  /home/ubuntu/immich/config

# Keep only last 30 days
find $BACKUP_DIR -type d -mtime +30 -exec rm -rf {} \;

echo "Backup completed: $DATE"
```

Make executable and schedule:

```bash
sudo chmod +x /usr/local/bin/homelab-backup.sh

# Add to crontab (daily at 2 AM)
sudo crontab -e
0 2 * * * /usr/local/bin/homelab-backup.sh
```

## Backup Storage

### Local Backup
```bash
# Create backup location
sudo mkdir -p /home/ubuntu/backups
sudo chown ubuntu:ubuntu /home/ubuntu/backups
```

### External Drive
```bash
# Mount external USB drive
sudo mkdir -p /mnt/backup
sudo mount /dev/sdb1 /mnt/backup

# Copy backups
sudo cp /home/ubuntu/backups/* /mnt/backup/
```

### Remote Server (SSH)
```bash
# One-time backup
scp ~/backups/* user@backup-server:/backups/

# Automated with SSH keys
# (Set up SSH key authentication first)
```

### Cloud Storage (Optional)
```bash
# Using rclone to sync to cloud
rclone sync ~/backups/ remote:homelab-backups/

# Encrypted backup example:
restic backup --repo s3:s3.amazonaws.com/bucket/homelab /home/ubuntu/
```

## Recovery Procedures

### Restore Database (Nextcloud)

```bash
# Stop Nextcloud
docker compose -f ~/homelab/cloud/nextcloud/docker-compose.yml down

# Restore database
gunzip < ~/backups/nextcloud-db.sql.gz | docker exec -i nextcloud-db mysql -u root -p$MYSQL_ROOT_PASSWORD

# Restart
docker compose -f ~/homelab/cloud/nextcloud/docker-compose.yml up -d
```

### Restore Files

```bash
# Extract backup
tar -xzf ~/backups/nextcloud-data.tar.gz -C /

# Set permissions
sudo chown -R www-data:www-data /var/www/html
```

### Complete Service Recovery

```bash
# 1. Restore database
gunzip < ~/backups/nextcloud-db.sql.gz | docker exec -i nextcloud-db mysql -u root -p$MYSQL_ROOT_PASSWORD

# 2. Restore configs
tar -xzf ~/backups/services-config.tar.gz -C /

# 3. Restart services
docker compose -f ~/homelab/cloud/nextcloud/docker-compose.yml restart
```

## Disaster Recovery Plan

### If Server Dies

1. **Install fresh OS**
   ```bash
   # Follow Initial Setup guide
   ```

2. **Install Docker**
   ```bash
   # Follow Docker installation
   ```

3. **Clone repository**
   ```bash
   git clone https://github.com/jitendhull/homelab.git ~/homelab
   ```

4. **Deploy services**
   ```bash
   cd ~/homelab/cloud/nextcloud
   docker compose up -d
   ```

5. **Restore databases**
   ```bash
   # From backup files
   ```

6. **Restore file data**
   ```bash
   # From backup storage
   ```

## Verification

Test backups monthly:

```bash
# Extract to temp location
mkdir /tmp/backup-test
tar -xzf ~/backups/latest.tar.gz -C /tmp/backup-test

# Verify files
ls -la /tmp/backup-test

# Cleanup
rm -rf /tmp/backup-test
```

## Best Practices

- ✅ Backup before major updates
- ✅ Test restore procedures regularly
- ✅ Keep backups in multiple locations
- ✅ Document backup schedule
- ✅ Set reminders to verify backups
- ✅ Encrypt sensitive backups
- ✅ Store off-site copies
- ❌ Don't keep backups on same disk
- ❌ Don't forget encryption
- ❌ Don't skip verification tests

## Backup Schedule Template

```
Daily (2 AM):
  - Database dumps
  - Config files
  - Small file changes

Weekly (Sunday 2 AM):
  - Full file backup
  - Verify databases

Monthly (1st of month):
  - Full system backup
  - Archive old backups
  - Test recovery
```

## Monitoring

Set calendar reminders:

- [ ] Every Monday: Verify backups completed
- [ ] Every month: Test recovery procedure
- [ ] Every quarter: Review backup strategy
- [ ] Yearly: Update documentation

## See Also

- [Security Best Practices](security.md)
- [Monitoring](monitoring.md)
- [Service guides](../services/) for service-specific backups
