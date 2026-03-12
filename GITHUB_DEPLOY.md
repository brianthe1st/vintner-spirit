# GitHub Deployment Guide

## Push to GitHub

### Step 1: Create a GitHub Repository

1. Go to https://github.com/new
2. Choose a repository name (e.g., `vintner-spirit`)
3. Set visibility (Public or Private)
4. **Don't** initialize with README, .gitignore, or license (we already have these)
5. Click "Create repository"

### Step 2: Push Your Code

Run these commands in your terminal:

```bash
# Add your GitHub repository as remote
git remote add origin https://github.com/YOUR_USERNAME/vintner-spirit.git

# Verify remote
git remote -v

# Push to GitHub
git push -u origin main
```

Replace `YOUR_USERNAME` with your GitHub username.

### Step 3: Verify Upload

Visit your repository on GitHub to confirm all files are uploaded.

---

## Deploy to Vercel (Recommended)

Vercel provides free hosting for frontend applications.

### Option A: Deploy from GitHub (Easiest)

1. Go to https://vercel.com/new
2. Click "Import Git Repository"
3. Select your GitHub repository
4. Configure project:
   - **Framework Preset**: Vite
   - **Build Command**: `npm run build`
   - **Output Directory**: `dist`
5. Add environment variables:
   - `VITE_SUPABASE_URL`
   - `VITE_SUPABASE_ANON_KEY`
   - `PAYPACK_API_KEY` (optional)
   - `PAYPACK_API_SECRET` (optional)
6. Click "Deploy"

### Option B: Deploy via CLI

```bash
# Install Vercel CLI
npm i -g vercel

# Login to Vercel
vercel login

# Deploy
vercel --prod
```

---

## Deploy to Netlify (Alternative)

1. Go to https://app.netlify.com/new
2. Import from GitHub
3. Build settings:
   - **Build command**: `npm run build`
   - **Publish directory**: `dist`
4. Add environment variables
5. Deploy

---

## Deploy Backend Server

Since this app uses an Express server with Supabase, you have options:

### Option 1: Vercel Serverless Functions

Vercel can handle API routes automatically.

1. Create `api/` folder at root
2. Move server endpoints to Vercel serverless functions format
3. Vercel will auto-deploy them

### Option 2: Railway.app (Recommended for Full-Stack)

Railway is perfect for Node.js + Supabase apps.

1. Go to https://railway.app
2. Create new project
3. Deploy from GitHub
4. Add environment variables
5. Railway will auto-detect Node.js and deploy

### Option 3: Render.com

Free alternative to Heroku.

1. Go to https://render.com
2. Create new Web Service
3. Connect GitHub repository
4. Configure:
   - **Build Command**: `npm install`
   - **Start Command**: `node server.ts` (or use tsx)
5. Add environment variables

---

## Environment Variables Setup

### On GitHub (for GitHub Actions)

1. Go to your repository Settings
2. Secrets and variables → Actions
3. Add repository secrets:
   - `VITE_SUPABASE_URL`
   - `VITE_SUPABASE_ANON_KEY`
   - `PAYPACK_API_KEY`
   - `PAYPACK_API_SECRET`

### On Vercel

1. Project Settings → Environment Variables
2. Add each variable for Production environment

### On Netlify

1. Site Settings → Environment Variables
2. Add each variable

---

## Continuous Deployment Setup

### Enable Auto-Deploy

Once connected to GitHub:

**Vercel:**
- Automatically deploys on every push to main branch
- Preview deployments for pull requests

**Netlify:**
- Auto-deploys on push to configured branch
- Deploy previews for PRs

**Railway/Render:**
- Auto-deploys on push to main branch

---

## Custom Domain (Optional)

### On Vercel:
1. Project Settings → Domains
2. Add your domain
3. Update DNS records as instructed

### On Netlify:
1. Domain Settings → Add custom domain
2. Follow DNS setup instructions

---

## Post-Deployment Checklist

After deploying:

- [ ] Test all API endpoints
- [ ] Verify Supabase connection
- [ ] Test user authentication
- [ ] Test product browsing
- [ ] Test checkout process
- [ ] Test admin panel
- [ ] Check environment variables are loaded
- [ ] Verify payment integration (PayPack)
- [ ] Test on mobile devices
- [ ] Check browser console for errors

---

## Useful GitHub Features

### GitHub Pages (Static Only)

If you only want to host the frontend:

1. Go to Settings → Pages
2. Source: Deploy from branch
3. Branch: main → /root
4. Save

Note: This won't work for full-stack apps with backend server.

### GitHub Actions (CI/CD)

Create `.github/workflows/deploy.yml` for automated testing and deployment.

Example workflow:

```yaml
name: Deploy to Production

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
      
      - name: Install dependencies
        run: npm install
      
      - name: Build
        run: npm run build
      
      - name: Deploy to Vercel
        uses: amondnet/vercel-action@v20
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-org-id: ${{ secrets.VERCEL_ORG_ID }}
          vercel-project-id: ${{ secrets.VERCEL_PROJECT_ID }}
          vercel-args: '--prod'
```

---

## Security Best Practices

1. **Never commit `.env` file** - Already in .gitignore ✓
2. **Use GitHub Secrets** for sensitive data
3. **Enable branch protection** on main branch
4. **Review pull requests** before merging
5. **Keep dependencies updated** - Run `npm audit` regularly
6. **Use environment-specific variables** for dev/staging/production

---

## Monitoring & Analytics

### Add These After Deployment:

1. **Vercel Analytics** - Built-in, free
2. **Supabase Logs** - Check database queries
3. **Sentry** - Error tracking
4. **Google Analytics** - User analytics (optional)

---

## Quick Commands Reference

```bash
# Check Git status
git status

# View commit history
git log --oneline

# Add and commit changes
git add .
git commit -m "Your message here"

# Push to GitHub
git push origin main

# Pull latest changes
git pull origin main

# Create new branch
git checkout -b feature-name

# Merge branch
git merge feature-name
```

---

## Troubleshooting

### "Permission denied (publickey)"
```bash
# Generate SSH key
ssh-keygen -t ed25519 -C "your_email@example.com"

# Add to GitHub
# Go to GitHub Settings → SSH and GPG keys → New SSH key
# Copy content of ~/.ssh/id_ed25519.pub
```

### "Repository not found"
- Check repository name and username
- Verify you have push permissions
- Use HTTPS instead of SSH if behind firewall

### Build Fails
- Check Node.js version (should be 18+)
- Clear cache: `rm -rf node_modules package-lock.json && npm install`
- Check environment variables are set correctly

---

## Next Steps

1. ✅ Push code to GitHub
2. ✅ Deploy to Vercel/Railway
3. ✅ Set up custom domain (optional)
4. ✅ Configure CI/CD pipeline
5. ✅ Set up monitoring
6. ✅ Share your live URL!

---

**Need Help?**
- Vercel Docs: https://vercel.com/docs
- Netlify Docs: https://docs.netlify.com
- Railway Docs: https://docs.railway.app
- GitHub Docs: https://docs.github.com
