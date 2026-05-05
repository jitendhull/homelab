# Prerequisites

Before you begin setting up your homelab, ensure you have everything needed.

## Hardware Requirements

### Minimum Viable Setup
- **CPU**: 2-core processor
- **RAM**: 4GB
- **Storage**: 20GB free space
- **Network**: Stable connection, static IP recommended

### Recommended Setup
- **CPU**: 4+ cores (Intel or AMD)
- **RAM**: 8GB or more
- **Storage**: 100GB+ (depends on media library size)
- **Network**: Gigabit Ethernet, static IP

## Software & OS

### Supported Operating Systems
- **Ubuntu 20.04 LTS or newer** (recommended)
- Debian 11+
- CentOS/RHEL 8+
- Proxmox (VM-based)
- Any Linux distro with Docker support

### Required Software

#### Docker
- Docker CE (Community Edition) 20.10+
- [Install Docker](https://docs.docker.com/engine/install/)

#### Docker Compose
- Docker Compose 2.0+
- Included with modern Docker installations
- Verify: `docker compose --version`

#### Basic Tools
- `curl` or `wget` (for file transfers)
- `git` (optional, for cloning/updating)
- Text editor (`nano`, `vim`, or `VS Code`)
- Terminal/SSH access

### Verification Checklist

Run these commands to verify your setup:

```bash
# Check Docker installation
docker --version
# Expected: Docker version 20.10 or higher

# Check Docker Compose
docker compose --version
# Expected: Docker Compose version 2.0 or higher

# Check system resources
free -h          # RAM usage
df -h            # Disk usage
uname -a          # OS information
```

## Network Requirements

### DNS & Domains (Optional but Recommended)
- Domain name (can use free domains like `.tk`)
- Access to domain registrar to manage DNS records
- or: Use local hostname/IP addresses

### Port Access
- Port 80 (HTTP) - for Nginx Proxy Manager
- Port 443 (HTTPS) - for Nginx Proxy Manager
- Port 81 (NPM Admin) - for managing reverse proxy
- Other service-specific ports (documented per service)

### Firewall Rules
- Open required ports on your firewall
- Configure port forwarding on your router (if accessing remotely)
- Consider security implications before opening ports

## Directory Structure

The homelab assumes the following directory structure on your host:

```
/home/ubuntu/
├── nextcloud/           # Nextcloud data
├── jellyfin/            # Jellyfin config & cache
├── immich/              # Immich photos
├── vaultwarden/         # Password manager data
├── alist/               # File storage
└── [other services]/    # Additional service data
```

!!! note
    All paths in compose files reference `/home/ubuntu/`. Adjust based on your actual username and preferred location. You can also use symbolic links to other storage locations.

## User Permissions

Ensure your user can run Docker without `sudo`:

```bash
# Add user to docker group
sudo usermod -aG docker $USER

# Apply group membership
newgrp docker

# Verify (no sudo needed)
docker ps
```

## Storage Planning

### For Media Services (Jellyfin, Music)
- Plan for 1-2GB per movie, 500MB per episode
- 50-200GB for a basic media library
- Larger setups may need dedicated storage drives

### For Photo Management (Immich)
- Plan for 0.5-2MB per photo (compressed)
- 100GB for ~100,000 photos

### For Database Services
- Usually minimal (10-50GB)
- MariaDB, PostgreSQL data

## Networking Setup

### Static IP (Recommended)
Assign your server a static IP address:

**Option 1: Router DHCP Static**
- Log into router
- Find your server's MAC address
- Assign a reserved/static IP

**Option 2: OS-level Static IP**
```bash
# Ubuntu/Debian
sudo nano /etc/netplan/00-installer-config.yaml
# Set static IP, gateway, DNS
sudo netplan apply
```

### Hostname
Set a meaningful hostname:

```bash
sudo hostnamectl set-hostname homelab
```

## Security Considerations

- [ ] Enable firewall (`ufw` on Ubuntu)
- [ ] Keep system packages updated
- [ ] Use strong passwords for all services
- [ ] Plan for SSL/TLS certificates (Let's Encrypt recommended)
- [ ] Document all passwords in a secure password manager
- [ ] Regular backups of important data

## Ready to Go?

Once you've verified all prerequisites:

→ **Next Step**: [Initial Setup](initial-setup.md)

---

!!! warning
    Do not skip the prerequisites. Many issues stem from improper Docker installation or insufficient system resources.
