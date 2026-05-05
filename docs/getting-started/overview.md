# Homelab Overview

## What is a Homelab?

A homelab is a collection of self-hosted applications and services running on your own hardware, giving you complete control and privacy over your data.

## The Philosophy

This homelab prioritizes:

- **Privacy** - Your data stays under your control
- **Reliability** - Services designed to run 24/7
- **Simplicity** - Docker Compose makes deployment straightforward
- **Scalability** - Easy to add new services without breaking existing ones

## Architecture Overview

All services run in **Docker containers**, orchestrated with **Docker Compose**. This approach provides:

- Isolated environments
- Easy updates and rollbacks
- Consistent deployments
- Simple resource management

### Service Categories

The homelab is organized into logical categories:

| Category | Purpose | Services |
|----------|---------|----------|
| **Cloud** | Personal cloud and photo storage | Nextcloud, Immich |
| **Entertainment** | Media and streaming | Jellyfin, Music |
| **Storage** | File sharing and sync | Alist |
| **Security** | Password and access management | Vault-Warden |
| **Reverse Proxy** | SSL/TLS and routing | Nginx Proxy Manager |
| **Docker** | Container management | Portainer, Dockge |
| **Game Servers** | Game hosting platform | Pterodactyl |
| **Tools** | Utilities and extras | Firefox, etc. |

## Tech Stack

- **Container Runtime**: Docker
- **Orchestration**: Docker Compose
- **Operating System**: Ubuntu (recommended)
- **Reverse Proxy**: Nginx Proxy Manager
- **Databases**: MariaDB, PostgreSQL, Redis (where needed)

## System Requirements

### Minimum
- CPU: 2 cores (4+ recommended)
- RAM: 4GB (8GB+ recommended)
- Storage: 20GB free (more for media)
- OS: Linux, macOS, or Windows (with Docker)

### Recommended
- CPU: 4+ cores
- RAM: 8GB+
- Storage: 100GB+ (depends on media library)
- Network: Stable connection with >10Mbps

## Next Steps

1. Check [Prerequisites](prerequisites.md) for your specific setup
2. Follow [Initial Setup](initial-setup.md) to prepare your system
3. Deploy services one by one from the [Services](../services/) section

---

*Tip: Take time to understand the basics before jumping into deployment. A solid foundation makes maintenance much easier!*
