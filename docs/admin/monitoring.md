# Monitoring

Set up monitoring for your homelab to track health and performance.

## Key Metrics

Monitor:
- **CPU Usage**: Should be <80%
- **Memory**: Should have >500MB free
- **Disk**: Keep >20% free space
- **Network**: Check bandwidth usage
- **Container Status**: All running

## Quick Monitoring

```bash
# Real-time stats
docker stats

# Memory usage
free -h

# Disk usage
df -h

# CPU usage
top -bn1 | head -20
```

## Tools

### Portainer (Web UI)
- Built-in monitoring
- Container stats
- Docker resource usage
- See [Portainer guide](../services/docker-stuff/portainer.md)

### Dockge (Web UI)
- Container management
- Stack monitoring
- Compose UI
- See [Dockge guide](../services/docker-stuff/dockge.md)

## Alerts (Optional)

Set up notifications:
- Disk space warning
- Container stop alert
- High CPU/memory
- Backup completion

## Logging

Review logs regularly:

```bash
# All service logs
docker compose logs

# Specific service
docker compose logs service-name -f

# Error logs
docker compose logs | grep -i error
```

## Health Checks

Services with health checks:
- Nextcloud
- Immich
- Jellyfin

Check status:
```bash
docker inspect --format='{{json .State.Health}}' container-name | jq
```

## Performance Tuning

If slow:
1. Check disk space
2. Check CPU/memory usage
3. Review database queries
4. Enable caching
5. Disable unused services

See [Troubleshooting](../troubleshooting.md) for common issues.
