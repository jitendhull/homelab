# Immich

**Immich** is a self-hosted photo and video management system with machine learning capabilities.

## Overview

| Aspect | Details |
|--------|---------|
| **Purpose** | Photo/video storage, organization, search |
| **Images** | `immich/immich-server`, `immich/immich-machine-learning` |
| **Port** | 2283 |
| **Database** | PostgreSQL |
| **Storage** | `/home/ubuntu/immich/` |

## Key Features

- Auto-upload from mobile apps
- Smart search with ML object detection
- Face recognition
- Timeline view
- Album organization
- Google Photos importer

## Prerequisites

- 4GB+ RAM (8GB+ for ML features)
- 50GB+ storage (depends on photo count)
- Docker and Docker Compose

## Installation

### 1. Create Directories

```bash
sudo mkdir -p /home/ubuntu/immich/{config,photos}
sudo chown -R ubuntu:ubuntu /home/ubuntu/immich
```

### 2. Deploy

```bash
cd ~/homelab/cloud/immich-app
docker compose up -d
```

### 3. Access

Visit `http://localhost:2283`

- Create account during first login
- Note: First user becomes admin

## Configuration

### Upload Photos

**Mobile:**
- Download Immich app from App Store/Play Store
- Configure server URL: `http://your-server:2283`
- Enable background upload

**Web Upload:**
- Login to web interface
- Click upload button
- Select photos/videos

### Enable ML Features

The machine learning container processes:
- Face detection
- Object recognition
- Smart search

This requires:
- Sufficient RAM (enable swap if needed)
- Initial processing time on first run

### Mobile App Setup

1. Server URL: `http://your-server:2283` or `https://your-domain.com`
2. Email: Your account email
3. Password: Your account password
4. Enable "Background backup"
5. Select folders to backup

## Maintenance

### Update

```bash
cd ~/homelab/cloud/immich-app
docker compose pull
docker compose up -d
```

### Backup Photos

```bash
sudo tar -czf ~/backups/immich-photos.tar.gz /home/ubuntu/immich/photos
```

### Check ML Status

```bash
docker compose logs immich_machine_learning
```

## Troubleshooting

### ML Container Crashes

- Increase RAM or enable swap
- Reduce image quality in settings
- Disable ML features if not needed

### Upload Fails

- Check server connectivity
- Verify disk space
- Check logs: `docker compose logs immich_server`

## Tips

- Use external storage for photos (mounted volume)
- Regularly backup photo library
- Enable redundancy for important photos
- Consider geo-redundant backup solution

## Next Steps

- 📱 Set up mobile app
- 🔍 Enable face detection
- 💾 Configure backups
- 🔐 Secure with reverse proxy

## See Also

- [Nextcloud](nextcloud.md) - Alternative cloud storage
- [Nginx Proxy Manager](../reverse-proxy/npm.md) - For remote access
