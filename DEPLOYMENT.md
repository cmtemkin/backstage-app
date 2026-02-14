# Backstage Deployment Guide

This guide walks you through deploying Charlie's Backstage Developer Portal to production.

## Architecture Overview

- **Backend:** Node.js server running on Railway/Render with managed PostgreSQL (port 7007)
- **Frontend:** Static site deployed to Vercel/Netlify
- **Database:** Managed PostgreSQL (from Railway/Render)
- **Catalog:** Reads from github.com/cmtemkin/backstage-catalog

## Prerequisites

1. GitHub Personal Access Token with `read:org` and `repo` scopes
2. Railway or Render account
3. Vercel or Netlify account

## Step 1: Deploy Backend to Railway

### Option A: Using Railway CLI

```bash
# Install Railway CLI
npm i -g @railway/cli

# Login to Railway
railway login

# Create new project
railway init

# Add PostgreSQL database
railway add -d postgres

# Set environment variables
railway variables set GITHUB_TOKEN="your_github_token_here"
railway variables set NODE_ENV="production"

# Deploy
railway up
```

### Option B: Using Railway Dashboard

1. Go to https://railway.app/new
2. Click "Deploy from GitHub repo"
3. Select `cmtemkin/backstage-app`
4. Add PostgreSQL database from the dashboard
5. Set environment variables:
   - `GITHUB_TOKEN` - Your GitHub PAT
   - `NODE_ENV` - `production`
   - `APP_BASE_URL` - Will be your Vercel/Netlify URL (e.g., `https://backstage.charlietemkin.com`)
   - `BACKEND_BASE_URL` - Railway backend URL (e.g., `https://backstage-backend.up.railway.app`)
6. Railway will auto-configure PostgreSQL variables (`POSTGRES_HOST`, `POSTGRES_PORT`, `POSTGRES_USER`, `POSTGRES_PASSWORD`)
7. Set the start command: `yarn workspace backend start`
8. Set the build command: `yarn install && yarn tsc && yarn build:backend`

## Step 2: Deploy Frontend to Vercel

### Using Vercel CLI

```bash
# Install Vercel CLI
npm i -g vercel

# Login to Vercel
vercel login

# Build the frontend
yarn build:frontend

# Deploy
cd packages/app/dist
vercel --prod
```

### Using Vercel Dashboard

1. Go to https://vercel.com/new
2. Import `cmtemkin/backstage-app`
3. Set build settings:
   - **Framework Preset:** Other
   - **Root Directory:** `packages/app`
   - **Build Command:** `yarn install && yarn build`
   - **Output Directory:** `dist`
4. Set environment variables:
   - `NODE_ENV` - `production`
   - `BACKEND_BASE_URL` - Your Railway backend URL

## Step 3: Update Environment Variables

After both deployments are live:

1. **Update Railway:**
   - Set `APP_BASE_URL` to your Vercel URL (e.g., `https://backstage.charlietemkin.com`)
   - Set `BACKEND_BASE_URL` to your Railway URL (e.g., `https://backstage-backend.up.railway.app`)

2. **Update Vercel:**
   - Set `BACKEND_BASE_URL` to your Railway backend URL

## Alternative: Deploy to Render

### Backend on Render

1. Go to https://dashboard.render.com/
2. Click "New +" → "Web Service"
3. Connect your GitHub account and select `cmtemkin/backstage-app`
4. Configure:
   - **Name:** backstage-backend
   - **Environment:** Node
   - **Build Command:** `yarn install && yarn tsc && yarn build:backend`
   - **Start Command:** `yarn workspace backend start`
5. Add PostgreSQL database from "New +" → "PostgreSQL"
6. Set environment variables (same as Railway above)

## Environment Variables Reference

### Required Variables

| Variable | Description | Example |
|----------|-------------|---------|
| `GITHUB_TOKEN` | GitHub PAT with read:org and repo scopes | `ghp_xxxxxxxxxxxx` |
| `POSTGRES_HOST` | PostgreSQL host | Auto-set by Railway/Render |
| `POSTGRES_PORT` | PostgreSQL port | `5432` |
| `POSTGRES_USER` | PostgreSQL username | Auto-set by Railway/Render |
| `POSTGRES_PASSWORD` | PostgreSQL password | Auto-set by Railway/Render |
| `BACKEND_BASE_URL` | Backend URL | `https://backstage-backend.railway.app` |
| `APP_BASE_URL` | Frontend URL | `https://backstage.charlietemkin.com` |
| `NODE_ENV` | Environment | `production` |

### Optional Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `LOG_LEVEL` | Logging level | `info` |
| `BACKEND_SECRET` | Secret for backend auth | Auto-generated |

## Custom Domain (Optional)

### Vercel
1. Go to your project settings
2. Add custom domain (e.g., `backstage.charlietemkin.com`)
3. Follow DNS configuration instructions

### Railway
1. Go to your project settings
2. Add custom domain (e.g., `api.backstage.charlietemkin.com`)
3. Configure DNS records

## Verification Checklist

After deployment:

- [ ] Backend is accessible at `BACKEND_BASE_URL/api/catalog/entities`
- [ ] Frontend loads and shows "Charlie's Developer Portal"
- [ ] Software catalog displays components from backstage-catalog repo
- [ ] All 14 components appear (4 systems, 8 apps, 6 libraries)
- [ ] GitHub Actions status visible for projects with workflows
- [ ] Search works across catalog entities
- [ ] TechDocs renders README content from repos
- [ ] Dependency graph shows connections between apps and shared services

## Troubleshooting

### Backend won't start
- Check `POSTGRES_*` variables are set correctly
- Verify `GITHUB_TOKEN` has correct scopes
- Check logs: `railway logs` or Render dashboard

### Frontend can't connect to backend
- Verify `BACKEND_BASE_URL` is set correctly in Vercel
- Check CORS settings in app-config.production.yaml
- Ensure backend is publicly accessible

### Catalog not loading
- Verify GitHub token has access to cmtemkin org
- Check backend logs for catalog processing errors
- Ensure backstage-catalog repo is public

### Database connection errors
- Verify PostgreSQL instance is running
- Check connection string format
- Ensure SSL is configured if required

## Monitoring

- **Railway/Render:** Built-in logging and metrics
- **Vercel:** Analytics and Web Vitals available in dashboard
- **Backstage:** Health check at `BACKEND_BASE_URL/healthcheck`

## Updating the Deployment

To deploy updates:

```bash
# Push to main branch
git push origin main

# Railway/Render will auto-deploy
# Vercel will auto-deploy

# Or manually trigger:
railway up  # for Railway
vercel --prod  # for Vercel
```

## Cost Estimates

- **Railway:** ~$5-10/month (includes PostgreSQL)
- **Render:** ~$7/month starter + $7/month PostgreSQL
- **Vercel:** Free for hobby projects
- **Total:** ~$5-20/month depending on usage
