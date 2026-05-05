# GitHub Pages Deployment Guide

Your homelab wiki is ready to deploy to GitHub Pages! Here's how to do it.

## Quick Setup (2 steps)

### Step 1: Push Your Code

```bash
cd ~/homelab

# Stage the new files
git add mkdocs.yml requirements.txt docs/ .github/ WIKI_SETUP.md

# Commit
git commit -m "Add comprehensive homelab wiki with MkDocs

- MkDocs configuration with Material theme
- 24 documentation files (3380+ lines)
- Service guides and admin documentation
- GitHub Actions workflow for auto-deployment
- Service template for easy expansion

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>"

# Push to GitHub
git push origin master
```

### Step 2: Enable GitHub Pages

1. Go to your repository on GitHub.com
2. Click **Settings** (gear icon at top right)
3. Scroll down to **Pages** section (left sidebar)
4. Under "Build and deployment":
   - **Source**: Select "GitHub Actions"
5. That's it! GitHub Actions will handle the build automatically

## What Happens Next

✅ GitHub Actions will:
1. Detect your push
2. Build the wiki using `mkdocs build`
3. Deploy to GitHub Pages automatically
4. Provide a URL to access your wiki

⏱️ First deploy takes ~1-2 minutes  
🔄 Future updates take ~30-60 seconds

## Access Your Wiki

Once deployed, your wiki will be available at:

```
https://jitendhull.github.io/homelab/
```

(Replace `jitendhull` with your GitHub username)

## How It Works

The GitHub Actions workflow (`.github/workflows/deploy-wiki.yml`):

1. **Triggers on**: Push to `master` or `main` branch
2. **Watches**: Changes to `docs/`, `mkdocs.yml`, or `requirements.txt`
3. **Builds**: Runs `mkdocs build` automatically
4. **Deploys**: Uploads to GitHub Pages

No manual build/upload needed!

## Auto-Update on Changes

After the initial setup, your wiki auto-updates:

```bash
# Make changes
nano docs/services/cloud/nextcloud.md

# Commit and push
git add docs/services/cloud/nextcloud.md
git commit -m "Update Nextcloud setup documentation"
git push

# ✅ Wiki auto-builds and deploys in ~1 minute
# No action needed!
```

## Test the Workflow

1. Go to your repo on GitHub
2. Click **Actions** tab
3. Look for "Deploy Wiki to GitHub Pages" workflow
4. Should show "✅ passed" in green

Click on it to see build details.

## Troubleshooting

### Wiki not appearing

**Check 1: Workflow Status**
- Go to Actions tab
- See if deploy-wiki.yml shows any errors
- Common issues:
  - Python dependency error → update `requirements.txt`
  - MkDocs build error → check `mkdocs.yml` syntax

**Check 2: Pages Settings**
- Settings → Pages
- Verify "Source" is set to "GitHub Actions"
- Not "Deploy from a branch"

**Check 3: DNS/Caching**
- First deploy can take 1-2 minutes
- Try clearing browser cache
- Wait a few minutes and refresh

### Build failing

```bash
# Test locally first
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
mkdocs build

# If it works locally, commit and push
# GitHub will use same process
```

## Customizing Your Wiki

### Change URL

To serve from a custom domain (e.g., `wiki.yourdomain.com`):

1. Add to `mkdocs.yml`:
   ```yaml
   site_url: https://wiki.yourdomain.com/
   ```

2. Add DNS record pointing to GitHub Pages
3. Configure custom domain in repo Settings → Pages

### Update Theme

Edit `mkdocs.yml`:

```yaml
theme:
  palette:
    primary: blue     # Change color
    accent: blue
```

Colors: red, pink, purple, indigo, blue, cyan, teal, green, lime, yellow, orange

## Adding New Content

The workflow watches for changes to:
- `docs/**` - Any file in docs directory
- `mkdocs.yml` - Config changes
- `requirements.txt` - Dependency changes

Just commit and push! Auto-deploy will handle the rest.

## Backup Locations

Your wiki source is now in:
1. **GitHub repo** (your main source)
2. **GitHub Pages** (published version)
3. **Local machine** (your working copy)

Extra safe! ✅

## Next Steps

1. ✅ Push code to GitHub
2. ✅ Enable GitHub Pages
3. ✅ Wait for first build (~2 mins)
4. ✅ Visit `https://yourusername.github.io/homelab/`
5. ✅ Share the link!

## Useful Links

- GitHub Pages Docs: https://pages.github.com/
- MkDocs Docs: https://www.mkdocs.org/
- Material Theme: https://squidfunk.github.io/mkdocs-material/
- GitHub Actions: https://docs.github.com/en/actions

---

**Status**: ✅ Ready to deploy  
**Workflow**: ✅ Configured  
**Next action**: Push code to GitHub

Happy documenting! 🚀📚
