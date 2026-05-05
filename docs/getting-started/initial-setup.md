# Initial Setup

This guide walks you through the initial setup of your homelab server.

## Step 1: System Update

Start with a fresh, updated system:

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y curl wget git nano vim
```

## Step 2: Install Docker

### Ubuntu/Debian

```bash
# Remove old versions if any
sudo apt remove docker docker.io containerd runc -y

# Install Docker's official repository
sudo apt install -y ca-certificates curl gnupg lsb-release
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin

# Verify installation
docker --version
docker compose --version
```

## Step 3: Configure Docker User Permissions

```bash
# Add your user to the docker group
sudo usermod -aG docker $USER

# Apply the new group membership (no logout required)
newgrp docker

# Verify (should not need sudo)
docker ps
```

## Step 4: Create Directory Structure

Create the base directories for all services:

```bash
# Create base directory
sudo mkdir -p /home/ubuntu

# Create service directories
sudo mkdir -p /home/ubuntu/{nextcloud,jellyfin,immich,vaultwarden,alist,portainer,dockge}

# Set permissions (adjust 'ubuntu' to your username if different)
sudo chown -R ubuntu:ubuntu /home/ubuntu
sudo chmod -R 755 /home/ubuntu
```

!!! note
    Replace `ubuntu` with your actual username if different. You can check your username with `whoami`.

## Step 5: Clone the Repository

```bash
# Clone the homelab repository
git clone https://github.com/jitendhull/homelab.git ~/homelab

# Navigate to the directory
cd ~/homelab
```

## Step 6: Configure Environment Variables

Each service uses environment variables for configuration. Create a `.env` file template:

```bash
# Example .env file structure (customize as needed)
cat > ~/.homelab.env << 'EOF'
# Nextcloud
MYSQL_ROOT_PASSWORD=your_secure_root_password
NEXTCLOUD_ADMIN_USER=admin
NEXTCLOUD_ADMIN_PASSWORD=your_secure_admin_password
NEXTCLOUD_TRUSTED_DOMAINS=localhost 127.0.0.1 homelab.local

# Immich
IMMICH_SERVER_URL=https://photos.your-domain.com
UPLOAD_LOCATION=/home/ubuntu/immich

# Jellyfin
TZ=Asia/Kolkata

# Vault-Warden (Password Manager)
DOMAIN=https://vault.your-domain.com
EOF
```

!!! warning
    **Never commit `.env` files to Git!** They contain sensitive credentials. The template above is for reference only.

## Step 7: Configure System Settings

### Enable systemd-resolved (DNS)

```bash
# Check current DNS
cat /etc/resolv.conf

# Enable systemd-resolved
sudo systemctl enable systemd-resolved
sudo systemctl start systemd-resolved
```

### Configure UFW Firewall (Ubuntu)

```bash
# Enable firewall
sudo ufw enable

# Allow SSH (if connecting remotely)
sudo ufw allow 22/tcp

# Allow HTTP/HTTPS
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

# Allow NPM admin interface
sudo ufw allow 81/tcp

# View rules
sudo ufw status
```

### Set Static IP (Ubuntu/Netplan)

```bash
# Edit netplan config
sudo nano /etc/netplan/00-installer-config.yaml
```

Example configuration:

```yaml
network:
  version: 2
  ethernets:
    eth0:
      dhcp4: no
      addresses:
        - 192.168.1.50/24
      gateway4: 192.168.1.1
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4]
```

Apply changes:

```bash
sudo netplan apply
```

## Step 8: Test Docker Installation

```bash
# Run hello-world container
docker run hello-world

# List images
docker images

# Check Docker daemon status
sudo systemctl status docker
```

## Step 9: Prepare for First Service Deployment

Before deploying services, understand the structure:

```
homelab/
├── cloud/
│   ├── nextcloud/
│   │   └── docker-compose.yml
│   └── immich/
│       └── docker-compose.yml
├── entertainment/
│   └── jellyfin/
│       └── docker-compose.yml
└── ... [other services]
```

Each service has its own `docker-compose.yml` file. Navigate to the service directory and use:

```bash
# Start a service
cd ~/homelab/cloud/nextcloud
docker compose up -d

# View logs
docker compose logs -f

# Stop a service
docker compose down
```

## Step 10: Configure Reverse Proxy (Optional but Recommended)

The Nginx Proxy Manager provides:
- Centralized SSL/TLS management
- Easy domain routing
- Let's Encrypt automation

Deploy first:
→ **Next**: [Nginx Proxy Manager Setup](../services/reverse-proxy/npm.md)

## Verification Checklist

- [ ] Docker installed and running
- [ ] User can run Docker without `sudo`
- [ ] Directory structure created
- [ ] Firewall configured
- [ ] Static IP assigned
- [ ] Ready to deploy services

## Common Issues

### Permission Denied Error
```bash
# If you still get permission errors:
sudo systemctl restart docker
newgrp docker
```

### Port Already in Use
```bash
# Find what's using the port
sudo lsof -i :80
sudo lsof -i :443

# Kill the process if needed
sudo kill -9 <PID>
```

### DNS Resolution Issues
```bash
# Test DNS
nslookup google.com
cat /etc/resolv.conf

# Restart systemd-resolved
sudo systemctl restart systemd-resolved
```

## Next Steps

1. ✅ Basic setup complete
2. 📚 Read [Service Guides](../services/)
3. 🚀 Deploy your first service
4. 🔒 Set up Nginx Proxy Manager
5. 📊 Configure monitoring

---

Ready to deploy your first service? Start with [Nextcloud](../services/cloud/nextcloud.md) or [Jellyfin](../services/entertainment/jellyfin.md)!
