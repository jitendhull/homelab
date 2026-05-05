# Service Template

This is a template for documenting new services. Copy and customize for each new service.

---

# Service Name

**Brief description** - What is this service and what does it do?

## Overview

| Aspect | Details |
|--------|---------|
| **Purpose** | What it does |
| **Image** | Docker image name |
| **Port** | Main access port |
| **Database** | If any (e.g., MySQL, PostgreSQL) |
| **Storage** | Data directory |

## Key Features

- Feature 1
- Feature 2
- Feature 3

## Prerequisites

- RAM requirement
- Storage requirement
- Other dependencies

## Installation

### 1. Create Directories

```bash
sudo mkdir -p /home/ubuntu/service-name/
sudo chown -R ubuntu:ubuntu /home/ubuntu/service-name
```

### 2. Deploy

```bash
cd ~/homelab/category/service-name
docker compose up -d
```

### 3. Access

Visit `http://localhost:PORT` or `https://service.yourdomain.com`

## Configuration

### Initial Setup

Steps to configure after deployment

### Important Settings

Key settings to configure

## Usage

### Common Tasks

### Examples

## Maintenance

### Update

```bash
cd ~/homelab/category/service-name
docker compose pull
docker compose up -d
```

### Backup

```bash
# Backup data
sudo tar -czf ~/backups/service-backup.tar.gz /home/ubuntu/service-name/
```

### Monitor

```bash
# Check status
docker compose ps

# View logs
docker compose logs -f

# Monitor resources
docker stats
```

## Troubleshooting

### Issue 1

Solution

### Issue 2

Solution

## Tips

- Tip 1
- Tip 2
- Tip 3

## Next Steps

- ✅ Service deployed
- 📋 Complete initial setup
- 🔐 Secure with reverse proxy
- 💾 Set up backups

## See Also

- [Related Service](../category/service.md)
- [Admin Guide](../../admin/)

---

## Using This Template

1. Copy this file: `cp service-template.md new-service.md`
2. Replace "Service Name" with actual name
3. Update all sections
4. Add to `mkdocs.yml` navigation
5. Create service directories: `mkdir -p /home/ubuntu/service-name`

## Template Sections

- **Overview** - Quick reference table
- **Key Features** - What makes it useful
- **Prerequisites** - What you need
- **Installation** - Step-by-step deployment
- **Configuration** - Post-install setup
- **Usage** - How to use it
- **Maintenance** - Keeping it running
- **Troubleshooting** - Fixing issues
- **Tips** - Best practices
- **Next Steps** - What to do next
