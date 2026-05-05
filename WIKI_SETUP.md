# Wiki Setup Summary

Your homelab now has a comprehensive GitBook-style wiki powered by MkDocs!

## What's Included

### 📚 Documentation Pages (24 files, 3380+ lines)

**Getting Started** (3 pages):
- Overview of homelab and architecture
- Prerequisites and hardware requirements
- Step-by-step initial setup guide

**Service Guides** (11+ pages):
- Nextcloud (cloud storage)
- Immich (photo management)
- Jellyfin (media streaming)
- Nginx Proxy Manager (reverse proxy)
- Vault-Warden (password manager)
- Alist (file storage)
- Plus placeholders for Music, Portainer, Dockge, Pterodactyl, Firefox

**Administration** (4 pages):
- Backup & Recovery strategies
- Monitoring setup
- Networking configuration
- Security best practices

**General Docs** (3 pages):
- Troubleshooting guide
- FAQ (20+ Q&A)
- Service template for adding new services

## Quick Start

### 1. Install Dependencies

```bash
pip install -r requirements.txt
```

### 2. View Locally

```bash
# Development server with auto-reload
mkdocs serve

# Visit: http://localhost:8000
```

### 3. Build for Deployment

```bash
# Creates static site in 'site/' directory
mkdocs build
```

## File Structure

```
homelab/
├── mkdocs.yml              # Wiki configuration
├── requirements.txt        # Python dependencies
├── docs/
│   ├── index.md           # Homepage
│   ├── README.md          # Building instructions
│   ├── getting-started/   # Setup guides
│   ├── services/          # Service documentation
│   ├── admin/             # Administration guides
│   ├── troubleshooting.md # Common issues
│   └── faq.md            # FAQ
└── site/                  # Output (after building)
```

## Key Features

✅ **GitBook-style Interface** - Material theme looks professional  
✅ **Easy to Edit** - Plain Markdown files  
✅ **Searchable** - Built-in search functionality  
✅ **Mobile Friendly** - Responsive design  
✅ **Dark Mode** - Toggle between light/dark themes  
✅ **Navigation** - Organized tabs and sidebar  
✅ **Scalable** - Easy to add more services  

## Next Steps

### Fill in Placeholder Services

The wiki has placeholders for remaining services. Use the template:

```bash
# Template location
docs/services/SERVICE-TEMPLATE.md

# Copy and customize for each service
cp docs/services/SERVICE-TEMPLATE.md docs/services/category/service.md
```

Then edit with service-specific information.

### Deploy the Wiki

**Option 1: GitHub Pages**
```bash
mkdocs build
git add site/
git commit -m "Deploy wiki"
git push
```
Then enable GitHub Pages in repository settings.

**Option 2: Self-hosted in Homelab**
```bash
# Add to docker-compose.yml
services:
  wiki:
    image: python:3.11-slim
    command: bash -c "pip install mkdocs mkdocs-material && mkdocs serve -a 0.0.0.0:8000"
    volumes:
      - ./:/docs
    ports:
      - "8080:8000"
```

**Option 3: Simple HTTP Server**
```bash
cd site/
python -m http.server 8000
# Visit: http://localhost:8000
```

### Access the Wiki

After deployment:
- Local: http://localhost:8000
- Domain: https://wiki.yourdomain.com (via reverse proxy)
- GitHub Pages: https://username.github.io/homelab

## Editing the Wiki

### Add New Page

1. Create markdown file: `docs/category/page.md`
2. Edit `mkdocs.yml` navigation
3. Test locally: `mkdocs serve`
4. Build and deploy

### Edit Existing Page

1. Edit markdown file directly
2. Refresh browser (development mode auto-reloads)
3. Build when ready to deploy

### Customize Theme

Edit `mkdocs.yml`:

```yaml
theme:
  name: material
  palette:
    primary: blue
    accent: blue
  logo: assets/logo.png
  favicon: assets/favicon.png
```

## Important Files

| File | Purpose |
|------|---------|
| `mkdocs.yml` | Configuration (theme, nav, plugins) |
| `requirements.txt` | Python dependencies |
| `docs/` | All markdown content |
| `docs/index.md` | Homepage |
| `docs/README.md` | Instructions for building |

## Maintenance

### Keep Documentation Updated

- Update when you add new services
- Update when you change configurations
- Update troubleshooting with new issues found
- Archive old versions if needed

### Regular Tasks

```bash
# Weekly: Review and update content
# Monthly: Update screenshots if UI changes
# Quarterly: Review structure, consolidate content
```

## Tips

✅ Write for newcomers (explain jargon)  
✅ Use clear headings and organization  
✅ Include step-by-step examples  
✅ Add troubleshooting sections  
✅ Keep content DRY (don't repeat)  
✅ Link between related pages  
✅ Use code blocks for commands  
✅ Update regularly  

## Sharing the Wiki

### Share Locally

```bash
# Make accessible to other devices
mkdocs serve -a 0.0.0.0:8000
# Access from: http://server-ip:8000
```

### Share Online

- GitHub Pages (free, public)
- Custom domain (via reverse proxy)
- Cloud hosting (AWS, Azure, GCP)
- Homelab (self-hosted)

## Resources

- **MkDocs**: https://www.mkdocs.org/
- **Material Theme**: https://squidfunk.github.io/mkdocs-material/
- **Markdown Guide**: https://www.markdownguide.org/
- **MkDocs Plugins**: https://github.com/mkdocs/mkdocs/wiki/MkDocs-Plugins

## Troubleshooting

### "No module named mkdocs"
```bash
pip install mkdocs mkdocs-material
```

### "Port 8000 already in use"
```bash
mkdocs serve -a 0.0.0.0:8001
```

### Build errors
```bash
rm -rf site/
mkdocs build --clean
```

## What Changed

### New Files
- `mkdocs.yml` - Wiki configuration
- `requirements.txt` - Dependencies
- `.github/copilot-instructions.md` - AI assistant guide
- `docs/` directory with 24 markdown files

### What to Git Push

```bash
git add mkdocs.yml requirements.txt docs/ .github/
git commit -m "Add comprehensive wiki and Copilot instructions"
git push
```

## Next: Fill in the Blanks

The wiki is structured and ready. Now:

1. ✅ Copy and customize SERVICE-TEMPLATE.md for remaining services
2. ✅ Fill in placeholders with service details
3. ✅ Add screenshots where helpful
4. ✅ Deploy to GitHub Pages or self-host
5. ✅ Share with users

## Questions?

- **Building**: See `docs/README.md`
- **Content**: See individual `.md` files
- **Configuration**: Edit `mkdocs.yml`
- **Deployment**: See setup options above

---

Your wiki is now ready to be the single source of truth for your homelab! 📚✨
