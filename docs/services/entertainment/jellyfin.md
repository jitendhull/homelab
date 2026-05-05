# Jellyfin

**Jellyfin** is a free media server for organizing and streaming your personal media library.

## Overview

| Aspect | Details |
|--------|---------|
| **Purpose** | Media streaming (movies, TV, music) |
| **Image** | `jellyfin/jellyfin:latest` |
| **Port** | 8096 |
| **Storage** | `/home/ubuntu/jellyfin/` |
| **Network Mode** | Host (for LAN discovery) |

## Key Features

- Stream movies and TV shows
- Music streaming
- Live TV support
- Automatic library organization
- Mobile and web apps
- No accounts required (optional)

## Prerequisites

- 2GB+ RAM
- 100GB+ storage (depends on library)
- Media library organized (optional)

## Installation

### 1. Create Directories

```bash
sudo mkdir -p /home/ubuntu/jellyfin/{config,cache}
sudo mkdir -p /home/ubuntu/media  # Your media files

sudo chown -R ubuntu:ubuntu /home/ubuntu/jellyfin
sudo chown -R ubuntu:ubuntu /home/ubuntu/media
```

### 2. Deploy

```bash
cd ~/homelab/entertainment/jellyfin
docker compose up -d
```

### 3. Initial Setup

Access at `http://localhost:8096` or `http://your-server-ip:8096`

First login will show setup wizard:
1. Display language
2. Network settings
3. User profile
4. Metadata download preferences

## Configuration

### Add Media Library

1. Dashboard → Libraries → Add Media Library
2. Select content type (Movies, Shows, Music)
3. Point to media folder: `/media/movies` (mounted in container)
4. Enable automatic organization if desired

### Organize Media Files

Jellyfin works best with organized media:

```
/home/ubuntu/media/
├── movies/
│   ├── Movie Title (Year)/
│   │   └── Movie Title (Year).mkv
│   └── Another Movie (Year)/
│       └── Another Movie (Year).mkv
├── shows/
│   └── Show Name/
│       ├── Season 01/
│       │   ├── Show Name - s01e01.mkv
│       │   └── Show Name - s01e02.mkv
│       └── Season 02/
└── music/
    └── Artist Name/
        └── Album Name/
            └── 01 - Song Name.mp3
```

### User Management

1. Dashboard → Users
2. Create users for family members
3. Set permissions per user
4. Configure parental controls if needed

## Accessing Jellyfin

### Web Browser
```
http://localhost:8096
http://your-server-ip:8096
https://jellyfin.yourdomain.com (with reverse proxy)
```

### Mobile Apps
- iOS: Jellyfin (App Store)
- Android: Jellyfin (Play Store)
- Configure server: `http://your-server:8096`

### Desktop Clients
- Windows: Jellyfin Media Player
- Linux: Jellyfin Media Player
- macOS: Infuse (3rd party)

## Performance Optimization

### Transcoding

Jellyfin can transcode video on-the-fly for compatibility:
- Enable in Dashboard → Playback
- Configure bitrate limits
- Set preferred codec

### Hardware Acceleration

If your CPU supports it:

1. Dashboard → Playback → Transcoding
2. Enable hardware acceleration
3. Select appropriate encoder (VAAPI, NVENC, etc.)

### Image Extraction

Disable if not needed to save resources:
- Dashboard → Library → Advanced
- Uncheck "Extract chapter images"

## Maintenance

### Update

```bash
cd ~/homelab/entertainment/jellyfin
docker compose pull
docker compose up -d
```

### Backup

```bash
# Backup config
sudo tar -czf ~/backups/jellyfin-config.tar.gz /home/ubuntu/jellyfin/config

# Media is backed up separately
```

### Monitor Performance

```bash
docker stats jellyfin
docker compose logs jellyfin | tail -50
```

## Troubleshooting

### Cannot Find Media Files

- Verify path in compose file matches `/media`
- Check file permissions: `ls -la /home/ubuntu/media`
- Manually refresh library: Dashboard → Libraries → Scan

### Transcoding Fails

- Check storage space: `df -h`
- Enable hardware acceleration if available
- Reduce quality settings

### Slow Performance

- Check CPU usage: `docker stats`
- Disable unnecessary plugins
- Enable hardware acceleration
- Reduce metadata download quality

## Tips

- Use categories to organize library
- Create collections for franchises
- Set up watch lists
- Enable "Mark watched" on all users
- Use external subtitles for non-English content

## Next Steps

- 🎬 Organize media library
- 👥 Create user accounts
- 📱 Install mobile apps
- 🔐 Set up reverse proxy for HTTPS
- 📊 Configure playback options

## See Also

- [Storage setup](../storage/alist.md) - File sharing for media
- [Nginx Proxy Manager](../reverse-proxy/npm.md) - Remote access
- [Music services](music.md) - Dedicated music streaming
