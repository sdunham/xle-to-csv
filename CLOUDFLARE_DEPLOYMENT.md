# Cloudflare Pages Deployment Guide

## Quick Start

Your XLE to CSV app is now ready to deploy to Cloudflare Pages!

## Option 1: Deploy via Git (Recommended)

1. **Push your code to GitHub, GitLab, or Bitbucket**

2. **Go to Cloudflare Pages**
   - Visit https://pages.cloudflare.com/
   - Log in with your Cloudflare account (or create one)

3. **Create a new project**
   - Click "Create a project"
   - Click "Connect to Git"
   - Select your repository

4. **Configure build settings**
   - **Framework preset:** SvelteKit
   - **Build command:** `npm run build`
   - **Build output directory:** `.svelte-kit/cloudflare`
   - **Root directory:** (leave empty)
   - **Environment variables:** None needed

5. **Deploy**
   - Click "Save and Deploy"
   - Your site will be live in a few minutes at `<your-project>.pages.dev`

## Option 2: Deploy via Wrangler CLI

1. **Install Wrangler**
   ```bash
   npm install -g wrangler
   ```

2. **Build your app**
   ```bash
   npm run build
   ```

3. **Deploy**
   ```bash
   npx wrangler pages deploy .svelte-kit/cloudflare --project-name=xle-to-csv
   ```

## Custom Domain (Optional)

After deployment, you can add a custom domain:
1. Go to your project in Cloudflare Pages
2. Click "Custom domains"
3. Add your domain
4. Update your DNS settings as instructed

## Environment Variables

This app doesn't require any environment variables for basic functionality.

## Notes

- The app is fully client-side (no server functions needed)
- Build time: ~5-10 seconds
- Deploys automatically on git push (if using Git integration)
- Free tier includes unlimited bandwidth and requests
