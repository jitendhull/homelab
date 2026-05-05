# Homelab Wiki - Building & Hosting

This directory contains the documentation for the homelab using MkDocs.

## Quick Start

### Prerequisites

- Python 3.8+
- pip (Python package manager)

### Build the Wiki

```bash
# Install dependencies
pip install -r requirements.txt

# Build static site
mkdocs build

# View site locally (development server)
mkdocs serve

# Access at: http://localhost:8000
```

### Structure

```
docs/
├── index.md                    # Homepage
├── getting-started/            # Setup guides
├── services/                   # Service documentation
├── admin/                      # Administration guides
├── troubleshooting.md          # Common issues
├── faq.md                      # Frequently asked questions
└── SERVICE-TEMPLATE.md         # Template for new services
```

## Adding New Pages

### Adding a New Service

1. **Copy the template**:
   ```bash
   cp docs/services/SERVICE-TEMPLATE.md docs/services/category/new-service.md
   ```

2. **Edit the new file** with service details

3. **Update `mkdocs.yml`** navigation:
   ```yaml
   nav:
     - Services:
       - Category:
         - New Service: services/category/new-service.md
   ```

4. **Build and test**:
   ```bash
   mkdocs serve
   ```

### Adding a New Admin Guide

1. **Create the file**:
   ```bash
   touch docs/admin/new-guide.md
   ```

2. **Add content** following other admin guides

3. **Update `mkdocs.yml`**:
   ```yaml
   - Administration:
     - New Guide: admin/new-guide.md
   ```

## Editing Pages

All pages are Markdown files (`.md`):

### Basic Markdown

```markdown
# Heading 1
## Heading 2
### Heading 3

**Bold** and *italic*

- Bullet point
- Another point

1. Numbered
2. List items

[Link text](url)

![][image.png](url or local path)
```

### Code Blocks

````markdown
```bash
docker compose up -d
```

```python
print("Hello")
```
````

### Tables

```markdown
| Column 1 | Column 2 |
|----------|----------|
| Cell 1   | Cell 2   |
| Cell 3   | Cell 4   |
```

### Admonitions (info boxes)

```markdown
!!! note
    This is a note

!!! warning
    This is a warning

!!! tip
    This is a helpful tip

!!! danger
    This is dangerous
```

## Hosting the Wiki

### Option 1: GitHub Pages

1. Push to GitHub
2. Go to repository Settings → Pages
3. Select "Deploy from a branch"
4. Choose branch (main/master) and folder (/root)
5. Wiki auto-builds and deploys

**Requires**: `gh-pages` branch setup (build locally, push compiled site)

### Option 2: Self-hosted

Serve from your homelab:

1. **Build site**:
   ```bash
   mkdocs build
   ```

2. **Serve with simple HTTP server**:
   ```bash
   cd site
   python -m http.server 8000
   ```

3. **Access**: `http://localhost:8000`

4. **Docker container** (optional):
   ```dockerfile
   FROM python:3.11-slim
   COPY requirements.txt .
   RUN pip install -r requirements.txt
   COPY . /docs
   WORKDIR /docs
   CMD ["mkdocs", "serve", "-a", "0.0.0.0:8000"]
   ```

### Option 3: Docker Compose

Add to your homelab:

```yaml
services:
  wiki:
    build:
      context: .
      dockerfile: docs/Dockerfile
    container_name: homelab-wiki
    restart: unless-stopped
    ports:
      - "8080:8000"
    volumes:
      - ./docs:/docs
    networks:
      - homelab-net
```

Deploy:
```bash
docker compose -f wiki.compose.yml up -d
```

Access: `http://localhost:8080`

## Deployment

### Automated Builds

Create `.github/workflows/deploy-wiki.yml`:

```yaml
name: Deploy Wiki

on:
  push:
    paths:
      - 'docs/**'
      - 'mkdocs.yml'

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-python@v4
        with:
          python-version: '3.11'
      - run: pip install -r requirements.txt
      - run: mkdocs build
      - uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./site
```

## Customization

### Theme Colors

Edit `mkdocs.yml`:

```yaml
theme:
  palette:
    - scheme: default
      primary: blue
      accent: blue
```

Available colors: red, pink, purple, indigo, blue, cyan, teal, green, lime, yellow, orange

### Logo & Favicon

Add to `mkdocs.yml`:

```yaml
theme:
  logo: assets/logo.png
  favicon: assets/favicon.png
```

Place files in `docs/assets/`

## Tips

- ✅ Write clear, concise documentation
- ✅ Use headers to organize content
- ✅ Include code examples
- ✅ Add tables for quick reference
- ✅ Use admonitions for important info
- ✅ Keep related pages linked
- ✅ Update regularly
- ❌ Don't be overly verbose
- ❌ Don't assume reader expertise
- ❌ Don't mix multiple languages

## Markdown Resources

- [Markdown Guide](https://www.markdownguide.org/)
- [MkDocs Documentation](https://www.mkdocs.org/)
- [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/)

## Building Locally

```bash
# Install dependencies once
pip install -r requirements.txt

# Serve locally (auto-reloads on changes)
mkdocs serve

# Visit: http://localhost:8000

# Build for production
mkdocs build

# Output in: site/
```

## Troubleshooting

### "mkdocs: command not found"

```bash
pip install mkdocs mkdocs-material
```

### "Port 8000 already in use"

```bash
mkdocs serve -a 0.0.0.0:8001
# Use different port
```

### Build errors

```bash
# Clear cache
rm -rf site/
mkdocs build --clean
```

## Support

For MkDocs issues:
- [MkDocs Docs](https://www.mkdocs.org/)
- [Material Docs](https://squidfunk.github.io/mkdocs-material/)
- [GitHub Issues](https://github.com/mkdocs/mkdocs/issues)

---

Happy documenting! 📚
