# 🚀 GitHub Deployment Guide - Vintner & Spirit

## ✅ What's Already Done

Your code is already pushed to GitHub: **https://github.com/brianthe1st/vintner-spirit**

---

## 📋 Deployment Options

### Option 1: GitHub Pages (Static Frontend Only) ⚠️ Limited

**Note**: GitHub Pages only hosts static files. Your Express server backend won't work directly.

#### Steps:

1. **Enable GitHub Pages**:
   - Go to your repository Settings
   - Navigate to "Pages"
   - Source: Deploy from a branch
   - Branch: main → `/docs` (or use GitHub Actions)

2. **Add Secrets to Repository**:
   - Go to Settings → Secrets and variables → Actions
   - Add these secrets:
     ```
     Name: VITE_SUPABASE_URL
     Value: https://mfjgohpwbztfvvrcqnoi.supabase.co
     
     Name: VITE_SUPABASE_ANON_KEY
     Value: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6Im1mamdvaHB3Ynp0ZnZ2cmNxbm9pIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NzMwNTg3MjgsImV4cCI6MjA4ODYzNDcyOH0.UnInqL7-h5bDyNxZdGBnfEe3WC_PTWLzJJPkOs0Pwb0
     ```

3. **GitHub Actions will auto-deploy** when you push to main branch

4. **Your site will be live at**:
   `https://brianthe1st.github.io/vintner-spirit/`

⚠️ **Limitation**: Backend API routes won't work on GitHub Pages. You'll need to deploy the server separately.

---

### Option 2: Vercel (Recommended - Full Support) ⭐ Best Choice

Vercel supports both frontend AND backend (serverless functions).

#### Quick Deploy:

1. **Go to**: https://vercel.com/new
2. **Import Git Repository**: Select `brianthe1st/vintner-spirit`
3. **Configure**:
   - Framework Preset: **Vite**
   - Build Command: `npm run build`
   - Output Directory: `dist`
   - Install Command: `npm install`

4. **Add Environment Variables**:
   ```env
   VITE_SUPABASE_URL=https://mfjgohpwbztfvvrcqnoi.supabase.co
   VITE_SUPABASE_ANON_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6Im1mamdvaHB3Ynp0ZnZ2cmNxbm9pIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NzMwNTg3MjgsImV4cCI6MjA4ODYzNDcyOH0.UnInqL7-h5bDyNxZdGBnfEe3WC_PTWLzJJPkOs0Pwb0
   PAYPACK_API_KEY=ID7ef50f2e-1659-11f1-aa4f-deadd43720af
   PAYPACK_API_SECRET=340cf2aac9c7239b3eb96b8783a67206da39a3ee5e6b4b0d3255bfef95601890afd80709
   PORT=3000
   ```

5. **Click Deploy** 🚀

**Live in 2-3 minutes!**

---

### Option 3: Netlify (Alternative) 

Similar to Vercel, great for full-stack apps.

1. **Go to**: https://app.netlify.com/new
2. **Import from GitHub**: Select your repository
3. **Build Settings**:
   - Build command: `npm run build`
   - Publish directory: `dist`
   - Functions directory: `netlify/functions` (for server)

4. **Add Environment Variables** (same as Vercel)

5. **Deploy**

---

### Option 4: Railway (Best for Node.js Backend) 🎯

Perfect for running your Express server.

1. **Go to**: https://railway.app
2. **New Project** → Deploy from GitHub
3. **Select**: `brianthe1st/vintner-spirit`
4. **Add Variables**:
   ```
   VITE_SUPABASE_URL
   VITE_SUPABASE_ANON_KEY
   PAYPACK_API_KEY
   PAYPACK_API_SECRET
   NODE_ENV=production
   PORT=3000
   ```
5. **Deploy**

Railway will automatically detect Node.js and deploy!

---

## 🔧 Configure for Production

### Update vite.config.ts for GitHub Pages

If deploying to GitHub Pages, update your base path:

```typescript
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import tailwindcss from '@tailwindcss/vite'

export default defineConfig({
  plugins: [react(), tailwindcss()],
  base: '/vintner-spirit/', // Add this for GitHub Pages
})
```

### Server Configuration for Production

For Vercel/Railway, your `server.ts` is already configured correctly with:
- Dynamic port assignment
- Environment variable loading
- Production static file serving

---

## 📊 Post-Deployment Checklist

After deploying, verify:

- [ ] Homepage loads correctly
- [ ] Products page displays items from Supabase
- [ ] Shopping cart works
- [ ] User registration/login works
- [ ] Admin panel is accessible (password: admin123)
- [ ] Orders can be placed
- [ ] Payment integration works (test mode)
- [ ] Mobile responsive design works
- [ ] No console errors

---

## 🔐 Security Best Practices

### GitHub Secrets

Never commit `.env` file! Use GitHub Secrets:

1. Repository Settings → Secrets and variables → Actions
2. Add all sensitive variables as secrets
3. Reference in workflows using `${{ secrets.SECRET_NAME }}`

### Environment Variables by Platform

**GitHub Actions**: Use `${{ secrets.NAME }}`
**Vercel**: Project Settings → Environment Variables
**Netlify**: Site Settings → Environment Variables
**Railway**: Project Settings → Variables

---

## 🔄 Continuous Deployment

All platforms support auto-deployment:

- **GitHub Actions**: Deploys on every push to main
- **Vercel**: Auto-deploys on git push
- **Netlify**: Auto-deploys on git push
- **Railway**: Auto-deploys on git push

### Preview Deployments

Vercel and Netlify create preview URLs for pull requests!

---

## 🌐 Custom Domain (Optional)

### On Vercel:
1. Project Settings → Domains
2. Add your domain
3. Update DNS records:
   ```
   Type: A
   Name: @
   Value: 76.76.21.21
   
   Type: CNAME
   Name: www
   Value: cname.vercel-dns.com
   ```

### On GitHub Pages:
1. Repository Settings → Pages
2. Custom domain: yourdomain.com
3. Create CNAME file in root:
   ```
   yourdomain.com
   ```

---

## 🐛 Troubleshooting

### Build Fails on GitHub Actions

**Error**: `VITE_SUPABASE_URL undefined`

**Solution**: Add secrets to repository (see above)

### Blank Page After Deploy

**Check**:
1. Browser console for errors
2. Network tab for failed API calls
3. Supabase credentials are correct
4. Supabase tables are created

### API Routes Not Working

**GitHub Pages**: Won't work - use Vercel/Railway instead
**Vercel/Railway**: Check environment variables

### CORS Errors

Update `server.ts` to add CORS headers:

```typescript
app.use((req, res, next) => {
  res.header('Access-Control-Allow-Origin', '*');
  res.header('Access-Control-Allow-Headers', '*');
  next();
});
```

---

## 📈 Monitoring

### Free Analytics Options:

1. **Vercel Analytics** - Built-in, privacy-friendly
2. **Supabase Logs** - Database query monitoring
3. **GitHub Insights** - Traffic and clones

### Error Tracking:

1. **Sentry** - Free tier available
2. **LogRocket** - Session replay
3. **Better Stack** - Log management

---

## 🎯 Recommended Setup

**Best Combination**:

- **Frontend + Backend**: Vercel (free, automatic SSL, CDN)
- **Database**: Supabase (free tier: 500MB, plenty for start)
- **Payments**: PayPack (as configured)
- **Domain**: Buy from Namecheap/Cloudflare, connect to Vercel

**Cost**: $0/month (all free tiers!)

---

## 🚀 Quick Deploy Commands

### Deploy to Vercel via CLI:

```bash
# Install Vercel CLI
npm i -g vercel

# Login
vercel login

# Deploy
vercel --prod
```

### Test Build Locally:

```bash
npm run build
npm run preview
```

---

## 📞 Need Help?

- **Vercel Docs**: https://vercel.com/docs
- **GitHub Actions**: https://docs.github.com/en/actions
- **Supabase**: https://supabase.com/docs
- **Your Repository**: https://github.com/brianthe1st/vintner-spirit

---

## ✨ Next Steps

1. ✅ Code is on GitHub
2. ⏭️ Choose deployment platform (recommend Vercel)
3. ⏭️ Add environment variables
4. ⏭️ Deploy
5. ⏭️ Test live site
6. ⏭️ Share with the world! 🎉

---

**Ready to deploy?** Let me know which platform you choose and I'll help you configure it! 🚀
