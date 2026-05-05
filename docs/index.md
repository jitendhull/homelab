# Homelab Wiki

Welcome to the comprehensive guide for setting up and managing your homelab infrastructure. This wiki contains everything you need to deploy, configure, and maintain all services.

## Quick Navigation

- **[Getting Started](getting-started/overview.md)** - New to this homelab? Start here
- **[Services](services/)** - Detailed guides for each service
- **[Administration](admin/)** - Maintenance, backup, and security
- **[Troubleshooting](troubleshooting.md)** - Common issues and solutions

## What's in This Homelab?

This homelab includes a comprehensive suite of self-hosted services:

### Cloud & Storage
- **Nextcloud** - Personal cloud storage and collaboration
- **Immich** - Self-hosted photo management
- **Alist** - File sharing and management

### Entertainment
- **Jellyfin** - Media server for movies, shows, and music
- **Music Services** - Music streaming solutions

### Security & Access
- **Vault-Warden** - Password manager
- **Nginx Proxy Manager** - Reverse proxy and SSL/TLS management

### Infrastructure
- **Portainer** - Docker container management UI
- **Dockge** - Docker Compose UI
- **Pterodactyl** - Game server hosting panel

### Tools & Utilities
- **Firefox** - Browser in a container

## Key Features

✅ **Docker-based** - All services run in containerized environments  
✅ **Organized** - Services grouped by category  
✅ **Recoverable** - Full documentation for recreating your setup  
✅ **Scalable** - Easy to add new services  
✅ **Monitored** - Health checks and status monitoring  

## Getting Started

### First Time Setup
1. Read the [Prerequisites](getting-started/prerequisites.md)
2. Follow the [Initial Setup Guide](getting-started/initial-setup.md)
3. Deploy services one by one from the [Services section](services/)

### Updating an Existing Setup
Jump directly to any [service guide](services/) to update or modify configurations.

## Documentation Structure

```
.
├── getting-started/       # Setup and prerequisites
├── services/              # Individual service guides
│   ├── cloud/             # Nextcloud, Immich
│   ├── entertainment/      # Jellyfin, Music
│   ├── storage/           # Alist
│   ├── security/          # Vault-Warden
│   ├── reverse-proxy/     # Nginx Proxy Manager
│   ├── docker-stuff/      # Management tools
│   ├── pterodactyl/       # Game servers
│   └── tools/             # Utilities
├── admin/                 # Administration tasks
├── troubleshooting.md     # Solutions to common issues
└── faq.md                 # Frequently asked questions
```

## How to Use This Wiki

- **Markdown files** are located in the `/docs` directory
- **Edit directly** in any text editor or Git interface
- **Structure** follows the navigation defined in `mkdocs.yml`
- **Add new pages** by creating a `.md` file and updating `mkdocs.yml`

## Repository Links

- **Repository**: [github.com/jitendhull/homelab](https://github.com/jitendhull/homelab)
- **Issues & Discussions**: Report problems or share ideas
- **Contributions**: All improvements welcome!

---

*Last updated: 2024* | Keep this wiki current as you evolve your homelab!
