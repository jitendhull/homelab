# Frequently Asked Questions

## General

### What's a homelab?

A homelab is a collection of self-hosted services and applications running on your own hardware. It gives you complete control over your data and services.

### Why self-host instead of using cloud?

**Privacy** - Your data stays with you  
**Control** - Full customization  
**Cost** - No recurring subscriptions  
**Learning** - Great for understanding IT infrastructure  
**Offline Access** - Services available even without internet  

### Is a homelab secure?

With proper configuration, yes. Security depends on:
- Strong passwords
- Regular updates
- Firewall configuration
- SSL/TLS encryption
- Regular backups
- Good practices

See [Security Best Practices](admin/security.md) for details.

### How much does it cost?

**Hardware**: $100-1000+ (one-time)  
**Electricity**: $10-50/month  
**Domain**: Free-$15/year  
**Software**: Free (all services in this setup)  

Much cheaper than cloud subscriptions long-term.

## Hardware & Setup

### What hardware do I need?

**Minimum**:
- 2-core CPU
- 4GB RAM
- 20GB storage
- Stable network

**Recommended**:
- 4+ cores
- 8GB+ RAM
- 100GB+ storage
- Gigabit ethernet

Old laptops and mini-computers work great!

### Can I run this on a Raspberry Pi?

Technically yes, but:
- ❌ Limited by CPU and RAM
- ❌ Slow for media transcoding
- ✅ Good for learning
- ✅ Good for minimal services

Better options: Used mini-PC, old laptop, or NUC.

## Services & Software

### Can I use different services?

Yes! This wiki covers popular services, but you can:
- Replace services (e.g., different photo manager)
- Add new services
- Remove services you don't need

Same Docker Compose principles apply.

### What's the difference between Nextcloud and Immich?

**Nextcloud**:
- General-purpose cloud storage
- Files, calendars, contacts
- More features

**Immich**:
- Photo-focused
- Better mobile auto-upload
- ML features
- Simpler interface

Use both or choose one.

## Performance

### How much storage do I need?

**Light use** (documents, few photos):
- 20-50GB

**Medium use** (some media, photos):
- 100-500GB

**Heavy use** (large media library):
- 500GB - 10TB+

More is always better!

### Why is my server slow?

Check:
1. CPU usage: `docker stats`
2. RAM usage: `free -h`
3. Disk space: `df -h`
4. Disk I/O: `iostat`

Common causes:
- Transcoding taking CPU
- ML processing using RAM
- Database queries slow
- Disk full

### Should I enable all services?

No:
- ❌ Don't run if not using
- ❌ Wastes resources
- ✅ Start minimal, add as needed
- ✅ Monitor resource usage

Deploy services one by one.

## Updates & Maintenance

### How often should I update?

Recommendation:
- **Security patches**: Immediately
- **Feature updates**: Weekly/monthly
- **Major versions**: When tested

```bash
docker compose pull && docker compose up -d
```

### Will updating break things?

Rarely if:
- ✅ You have backups
- ✅ You test updates
- ✅ You follow migration guides

Always have a rollback plan.

## Backup & Recovery

### How often should I backup?

**Minimum**: Monthly  
**Recommended**: Weekly  
**Critical data**: Daily  

Automate with cron jobs.

### Can I backup to cloud?

Yes:
- AWS S3
- Google Cloud Storage
- Backblaze B2
- Any cloud provider with API

See [Backup & Recovery](admin/backup-recovery.md).

### How do I test backups?

```bash
# Extract to test location
tar -xzf backup.tar.gz -C /tmp/test

# Verify files exist
ls -la /tmp/test

# Restore to production if good
```

Test monthly!

## Troubleshooting

### Where do I look for errors?

```bash
# Service logs
docker compose logs service-name

# Container logs
docker logs container-name

# System logs
journalctl -u docker
```

### How do I get help?

1. Check [Troubleshooting](troubleshooting.md) guide
2. Search service documentation
3. Check GitHub issues
4. Ask on community forums

## Still Have Questions?

- 📚 Check relevant service documentation
- 🔍 Search GitHub issues
- 💬 Join homelab communities
- 📧 Ask on forums

Good luck with your homelab!
