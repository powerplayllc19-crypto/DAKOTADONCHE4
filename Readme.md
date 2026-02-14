# GitHub Pages Deployment Guide for Dakota Donche Mechanical

---

## Step 1: Prerequisites

Make sure you have these installed on your computer:
- **Node.js** (v16 or higher) - Download from https://nodejs.org/
- **Git** - Download from https://git-scm.com/
- A **GitHub account** - Sign up at https://github.com/

---

## Step 2: Option A - Build from Source (Recommended)

### 2.1 Get the Source Files

1. **Download all project files** from where you have them, OR if you received a zip file:
   - Extract the zip to a folder on your computer (e.g., `C:\Projects\dakota-mechanical`)

2. **Open Command Prompt/Terminal** and navigate to the project folder:
   ```bash
   cd C:\Projects\dakota-mechanical   # Windows
   cd /Users/yourname/projects/dakota-mechanical   # Mac/Linux
   ```

### 2.2 Install Dependencies

```bash
npm install
```
This will install all required packages (React, Vite, Tailwind CSS, etc.)

### 2.3 Build the Production Version

```bash
npm run build
```

This will create a `dist/` folder containing:
- `index.html` (single file with everything embedded)
- That's it! Everything is in one file.

### 2.4 Test Locally (Optional but Recommended)

```bash
npm run preview
```

Then open your browser to `http://localhost:4173` to see if it works correctly.

---

## Step 3: Create a GitHub Repository

1. Go to https://github.com/ and log in
2. Click the **"+"** icon in the top right → **"New repository"**
3. Fill in the details:
   - **Repository name:** `dakota-mechanical` (or any name you like)
   - **Description:** "Dakota Donche Mechanical - Certified Master Mechanic"
   - Set to **Public** (required for free GitHub Pages)
   - Click **"Create repository"**

---

## Step 4: Push Your Code to GitHub

Back in your Command Prompt/Terminal:

```bash
# Initialize git (if not already done)
git init

# Add all files
git add .

# Commit the files
git commit -m "Initial commit - Dakota Donche Mechanical website"

# Rename main branch
git branch -M main

# Connect to your GitHub repository
# Replace YOUR_USERNAME and YOUR_REPO_NAME below:
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git

# Push to GitHub
git push -u origin main
```

Example:
```bash
git remote add origin https://github.com/johndoe/dakota-mechanical.git
git push -u origin main
```

---

## Step 5: Set Up GitHub Pages

### Method A: Deploy from the dist folder (Recommended)

We'll use a GitHub Action to automatically build and deploy your site.

#### 5.1 Create a GitHub Actions Workflow File

Create a new file at `.github/workflows/deploy.yml` in your project:

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [ main ]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: true

jobs:
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Build
        run: npm run build

      - name: Setup Pages
        uses: actions/configure-pages@v4

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: './dist'

      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

#### 5.2 Push the workflow file to GitHub

```bash
git add .github/workflows/deploy.yml
git commit -m "Add GitHub Pages deployment workflow"
git push
```

#### 5.3 Enable GitHub Pages in Repository Settings

1. Go to your GitHub repository
2. Click **"Settings"** (top tab)
3. On the left sidebar, click **"Pages"** (under "Code and automation")
4. Under **"Source"**, select **"GitHub Actions"**
5. Save the settings

#### 5.4 Trigger the Deployment

1. Go to the **"Actions"** tab in your repository
2. Click on the latest workflow run
3. Click **"Re-run all jobs"** if it didn't start automatically
4. Wait for the build to complete (usually 1-2 minutes)

#### 5.5 Get Your Live URL

After successful deployment:
1. Go back to **Settings → Pages**
2. You'll see: "Your site is live at `https://YOUR_USERNAME.github.io/YOUR_REPO_NAME/`"
3. Click the link to view your live website!

---

## Step 6: Alternative Method - Manual Deploy (Simpler)

If you don't want to use GitHub Actions:

### 6.1 Build the project locally

```bash
npm run build
```

### 6.2 Create a `docs` folder

```bash
# Copy dist contents to docs folder
mkdir docs
cp dist/* docs/
```

Or manually copy the `index.html` file from `dist/` to a new folder named `docs/`

### 6.3 Push to GitHub

```bash
git add docs/
git commit -m "Add built files for deployment"
git push
```

### 6.4 Configure GitHub Pages

1. Go to your repository **Settings → Pages**
2. Under **"Source"**, select **"Deploy from a branch"**
3. Select branch: **main**
4. Select folder: **/docs**
5. Click **Save**
6. Wait 1-2 minutes, then refresh the page to see your URL

---

## Troubleshooting Common Issues

### Issue: Blank Page After Deployment

**Solution:**
1. Open your browser's **Developer Tools** (F12 or Ctrl+Shift+I)
2. Check the **Console** tab for errors
3. Most common issue: Path problems with assets

### Issue: 404 Error

**Solution:**
1. Make sure you selected the correct branch and folder in GitHub Pages settings
2. Wait 2-3 minutes after deployment
3. Try clearing your browser cache

### Issue: CSS/Styling Not Loading

**Solution:**
1. Make sure you used `npm run build` (not `npm run dev`)
2. Check that the `dist/index.html` file is complete (should be ~300KB)
3. The vite-plugin-singlefile embeds everything, so if it's built correctly, it should work

---

## Testing Your Deployment

After deployment, test these critical features:

1. ✅ Page loads completely (no blank screen)
2. ✅ All images display correctly
3. ✅ Click-to-call phone number works: (505) 317-8136
4. ✅ Navigation menu works on mobile
5. ✅ Contact form displays correctly
6. ✅ Gallery images load
7. ✅ Testimonials carousel works

---

## Custom Domain (Optional)

If you want a custom domain (e.g., www.dakotadonchemechanical.com):

1. Purchase a domain from a registrar (GoDaddy, Namecheap, etc.)
2. In your GitHub repo **Settings → Pages**:
   - Enter your custom domain under "Custom domain"
   - Click **Save**
3. In your domain registrar's DNS settings:
   - Add an **A record** pointing to: `185.199.108.153`
   - Add another **A record** pointing to: `185.199.109.153`
   - Add another **A record** pointing to: `185.199.110.153`
   - Add another **A record** pointing to: `185.199.111.153`
   - Add a **CNAME record** for `www` pointing to: `YOUR_USERNAME.github.io`
4. Wait for DNS propagation (can take up to 24 hours)

---

## Updating Your Website

When you make changes:

```bash
# 1. Make your changes to the source files

# 2. Push to GitHub
git add .
git commit -m "Update: description of changes"
git push

# 3. The GitHub Action will automatically rebuild and deploy!
```

---

## Quick Checklist Before Deploying

- [ ] All images are optimized (under 500KB each)
- [ ] Phone number is correct: (505) 317-8136
- [ ] Service area is set to Albuquerque
- [ ] Contact form is working (consider adding Formspree for functionality)
- [ ] All links are working
- [ ] Website looks good on mobile
- [ ] SSL/HTTPS is enabled (automatic with GitHub Pages)

---

## Need Help?

If you're still having issues:

1. Check that your `package.json` has the correct build script:
   ```json
   "scripts": {
     "dev": "vite",
     "build": "tsc && vite build",
     "preview": "vite preview"
   }
   ```

2. Verify your `vite.config.ts` has the singlefile plugin

3. Make sure the `dist/index.html` file exists and is not empty

4. Try opening `dist/index.html` directly in your browser to confirm it works locally

---

## Final Deployed URL Format

Your website will be available at:
```
https://YOUR_GITHUB_USERNAME.github.io/YOUR_REPOSITORY_NAME/
```

Example:
```
https://johnsmith.github.io/dakota-mechanical/
```

Congratulations! Your professional mechanic website will be live and accessible to customers!
