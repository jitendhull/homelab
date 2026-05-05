# Security Best Practices

Essential security measures for your homelab.

## Access Control

### Strong Passwords

Use 16+ characters with:
- Uppercase letters
- Lowercase letters
- Numbers
- Special characters

Examples:
- ❌ `password123`
- ✅ `Tr0pic@lThund3r$tor!`

Tools: Bitwarden, 1Password, KeePass

### Change Default Passwords

Immediately change:
- Nextcloud admin password
- NPM admin password
- Database root password
- Any service default credentials

### Unique Passwords

Never reuse passwords:
- ✅ Unique password per service
- ❌ Same password across services

Use password manager.

### Two-Factor Authentication

Enable where available:
- Nextcloud: Admin Settings → Security → 2FA
- Nginx Proxy Manager: Basic auth + IP whitelist
- Services: Enable TOTP if supported

## Network Security

### Firewall

Ubuntu UFW setup:

```bash
# Enable firewall
sudo ufw enable

# Allow SSH (if remote)
sudo ufw allow 22/tcp

# Allow HTTP/HTTPS
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

# Allow NPM admin
sudo ufw allow 81/tcp

# View rules
sudo ufw status verbose
```

### SSL/TLS Encryption

Always use HTTPS:
- ✅ Automatic via Nginx Proxy Manager + Let's Encrypt
- ✅ Encrypt traffic end-to-end
- ❌ Never use plain HTTP for remote access

### VPN Access

Most secure option:

1. Set up OpenVPN or WireGuard
2. Connect to VPN first
3. Access services locally
4. No ports exposed to internet

### IP Whitelisting

Restrict access to known IPs:
- NPM Admin: Add IP whitelist
- SSH: Restrict to specific IPs
- Services: Configure access lists

## Data Protection

### Database Passwords

Environment variables only:

```yaml
# ✅ Use environment variables
environment:
  MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}

# ❌ Never hardcode
environment:
  MYSQL_ROOT_PASSWORD: actual_password
```

Store in `.env` file:
```bash
# .env (NOT in git)
MYSQL_ROOT_PASSWORD=secure_password_here
```

### Sensitive Files

Never commit to Git:
- `.env` files
- Keys and certificates
- Passwords
- API tokens

Add to `.gitignore`:
```
.env
.env.local
docker-compose.override.yml
secrets/
```

### Backups

Encrypt sensitive backups:

```bash
# Encrypt backup
tar -czf - /home/ubuntu/ | openssl enc -aes-256-cbc -out backup.tar.gz.enc

# Decrypt
openssl enc -d -aes-256-cbc -in backup.tar.gz.enc | tar -xz
```

## Regular Maintenance

### Update Services

```bash
# Check for updates
docker images

# Pull latest
docker compose pull

# Update safely
docker compose up -d

# Review logs
docker compose logs
```

Schedule:
- Security patches: Immediately
- Feature updates: Monthly
- Major versions: When stable

### Monitor Logs

```bash
# View logs regularly
docker compose logs -f

# Check for errors
docker compose logs | grep -i error

# Audit access logs
docker exec npm_app tail -f /var/log/nginx/access.log
```

### Test Backups

```bash
# Monthly backup tests
tar -xzf backup.tar.gz -C /tmp/test
ls -la /tmp/test/
rm -rf /tmp/test
```

## Vulnerability Scanning

### Docker Image Scanning

```bash
# Using Trivy (free tool)
curl -sfL https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/install.sh | sh -s -- -b /usr/local/bin

trivy image nextcloud:latest
```

### Dependencies

Keep updated:
- Docker version
- Docker Compose version
- Base OS (Ubuntu, Debian)
- Service images

## Remote Access Security

### Reverse Proxy Configuration

Nginx Proxy Manager settings:
- ✅ Block common exploits: Enabled
- ✅ SSL certificate: Valid and renewed
- ✅ Basic auth: For sensitive services
- ❌ Direct access on custom ports

### Domain Security

- ✅ Use DNSSEC if available
- ✅ Monitor DNS changes
- ✅ Use domain registrar lock
- ❌ Don't expose IP in DNS

### Port Forwarding

If using:
- ✅ Only forward 80, 443 to reverse proxy
- ❌ Never forward admin panels (81)
- ❌ Never forward database ports
- ❌ Never forward SSH directly

## User Management

### Nextcloud Users

Best practices:
- ✅ Create per-user accounts
- ✅ Disable users instead of deleting
- ✅ Set appropriate quotas
- ✅ Use groups for permissions
- ❌ Don't share single account

### Admin Accounts

- ✅ Limit admin users
- ✅ Use strong passwords
- ✅ Enable 2FA if available
- ✅ Log admin actions
- ❌ Don't use for regular access

## Documentation

### Keep Records

Document:
- Admin passwords (encrypted)
- Service URLs
- Backup locations
- Recovery procedures
- Security policies

Store in:
- Password manager
- Encrypted note
- Offline document

### Access Control List

Keep updated:
- Who has access to what
- When access was granted
- Review periodically

## Incident Response

### If Compromised

1. Identify affected service
2. Stop the service: `docker compose down`
3. Check logs for intrusion
4. Review backups
5. Restore from clean backup
6. Update all passwords
7. Review security

### Regular Security Audits

Quarterly:
- [ ] Review access logs
- [ ] Test backups
- [ ] Update all services
- [ ] Change admin passwords
- [ ] Review user accounts
- [ ] Check firewall rules

## Compliance

### GDPR (if applicable)

If hosting others' data:
- Privacy policy
- Data handling procedures
- Deletion procedures
- Breach notification plan
- Data retention policy

### Legal

- Check local laws
- Follow ISP ToS
- No illegal content
- Respect copyright
- Document practices

## Security Checklist

- [ ] Strong passwords set
- [ ] Default credentials changed
- [ ] 2FA enabled
- [ ] Firewall configured
- [ ] SSL/TLS enabled
- [ ] Backups encrypted
- [ ] .env in .gitignore
- [ ] VPN configured
- [ ] IP whitelisting enabled
- [ ] Regular updates scheduled
- [ ] Logs monitored
- [ ] Backup tested
- [ ] Documentation updated

## Quick Reference

```bash
# Check open ports
sudo ss -tuln | grep LISTEN

# View firewall rules
sudo ufw status verbose

# Check user accounts
cut -d: -f1 /etc/passwd

# Review Docker containers
docker ps -a

# Monitor system
htop
```

## Resources

- Docker Security: https://docs.docker.com/engine/security/
- OWASP Top 10: https://owasp.org/www-project-top-ten/
- Docker Bench: https://github.com/docker/docker-bench-security
- Nextcloud Security: https://docs.nextcloud.com/server/latest/admin_manual/security/

## Remember

Security is a process, not a destination. Regular reviews and updates are essential!
