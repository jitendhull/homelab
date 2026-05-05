# Copilot Instructions for Homelab Repository

## Architecture Overview

This is a self-hosted homelab infrastructure project organized around Docker Compose configurations. The repository is structured by service category, with each service living in its own directory containing a `docker-compose.yml` file and related data/config volumes.

### Directory Organization

```
homelab/
├── cloud/              # Cloud storage and sync (Nextcloud, Immich)
├── docker-stuff/       # Docker management tools (Dockge, Portainer)
├── entertainment/      # Media services (Jellyfin, Music streaming)
├── pterodactyl/        # Game server hosting panel
├── reverse-proxy/      # Reverse proxy & SSL (Nginx Proxy Manager)
├── security/           # Password management (Vault-Warden)
├── storage/            # File sharing (Alist)
└── tools/              # Utility containers (Firefox, etc.)
```

## Key Conventions

### Docker Compose Patterns

1. **Service Naming**: Container names follow the pattern `{service-name}` or `{service-name}-{component}` (e.g., `nextcloud-db`, `jellyfin`)
2. **Volume Paths**: Mount paths are typically rooted at `/home/ubuntu/{service-name}/`. Example: `/home/ubuntu/nextcloud/data:/var/www/html/data`
3. **Networking**: Most services use a dedicated bridge network (e.g., `nextcloud-net`). Some use `host` mode for LAN discovery (e.g., Jellyfin)
4. **Environment Variables**: Services use environment variable interpolation with defaults. Pattern: `${VAR_NAME:-default_value}`
5. **Timezone**: Services explicitly set timezone to `Asia/Kolkata`
6. **Healthchecks**: Multi-service containers (like Nextcloud) include healthchecks with dependencies (`depends_on: condition: service_healthy`)
7. **Restart Policy**: All services use `restart: unless-stopped`

### Configuration Management

- Services accept environment variables for customization (e.g., `MYSQL_ROOT_PASSWORD`, `NEXTCLOUD_ADMIN_USER`)
- Volume mounting paths reference absolute paths on the host (e.g., `/home/ubuntu/...`)
- Sensitive values should be passed via environment variables, not hardcoded in compose files

## Common Tasks

### Deploy or Update a Service

```bash
cd {service-directory}
docker-compose up -d                    # Start service
docker-compose logs -f                  # View logs
docker-compose down                     # Stop and remove containers
docker-compose pull && docker-compose up -d  # Update to latest image
```

### Add a New Service

1. Create a directory under the appropriate category: `{category}/{service-name}/`
2. Create `docker-compose.yml` with:
   - Explicit `container_name` for each service
   - Environment variables with defaults for configuration
   - Volume mounts using `/home/ubuntu/{service-name}/` pattern
   - Custom network (bridge) unless host networking is required
   - Healthcheck if multi-container
   - `restart: unless-stopped` policy
3. Document any required pre-setup (directories, permissions) in comments

### Debugging a Service

```bash
cd {service-directory}
docker-compose logs {container-name}       # View logs
docker-compose exec {container-name} sh    # Access container shell
docker ps | grep {service-name}            # Check if running
docker inspect {container-name}            # Inspect configuration
```

### Check Service Health

```bash
docker-compose ps              # Show status of all services in compose file
docker stats                   # Monitor resource usage
```

## Notes for Future Development

- When modifying compose files, ensure healthchecks are updated if service logic changes
- Volume paths are host-specific (`/home/ubuntu/...`); consider how to make these portable for documentation
- Some services include explicit network configuration while others use defaults; maintain consistency when adding new services
- Environment variable defaults should balance security with ease of deployment
