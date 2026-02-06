# ⚡ GitHub Actions Setup - Quick Start

## 5-Minute Setup Guide

### Step 1: Create Docker Hub Access Token

1. Go to [Docker Hub](https://hub.docker.com)
2. Sign in to your account
3. Click your **Profile Icon** → **Account Settings**
4. Go to **Security** (left sidebar)
5. Click **Create access token**
6. Enter name: `github-actions`
7. Keep it Private
8. Click **Create**
9. **Copy the token** (looks like: `dckr_pat_xxxxxxxxxxxxx`)

### Step 2: Add GitHub Secrets

1. Go to your GitHub repository
2. Click **Settings** (top menu)
3. Click **Secrets and variables** → **Actions** (left sidebar)
4. Click **New repository secret** (green button)

**Add Secret #1:**
- Name: `DOCKER_USERNAME`
- Secret: `your_docker_hub_username` (e.g., `aivis`)
- Click **Add secret**

**Add Secret #2:**
- Name: `DOCKER_PASSWORD`
- Secret: `paste_the_token_you_copied` (e.g., `dckr_pat_xxxx`)
- Click **Add secret**

### Step 3: Verify Workflow File

Check that `.github/workflows/build-and-deploy.yml` exists in your repo:

```bash
# If using Git command line
git add .github/workflows/build-and-deploy.yml
git commit -m "Add GitHub Actions workflow"
git push origin main
```

### Step 4: Test the Workflow

1. Make a small change to any file
2. Push to main branch:
   ```bash
   git add .
   git commit -m "Test CI/CD workflow"
   git push origin main
   ```
3. Go to GitHub repo → **Actions** tab
4. You should see your workflow running!

### Step 5: Monitor the Build

1. Click on the running workflow
2. Click on the **build** job
3. Watch the steps execute:
   - ✅ Checkout code
   - ✅ Login to Docker Hub
   - ✅ Build Docker Image
   - ✅ Push to Docker Hub

---

## 🎯 What Happens Next?

After successful build:

1. **Docker Hub gets your image**
   ```
   https://hub.docker.com/r/your_username/aivihub-api
   ```

2. **Anyone can pull it**
   ```bash
   docker pull your_username/aivihub-api:latest
   docker run -p 8000:8000 your_username/aivihub-api:latest
   ```

3. **You can deploy to AWS EC2, Kubernetes, etc.**

---

## 🧪 Testing Locally

Before pushing, test locally:

```bash
# Build the image
docker build -t your_username/aivihub-api:latest .

# Test the image
docker run -p 8000:8000 your_username/aivihub-api:latest

# Access at http://localhost:8000
```

---

## ❌ Common Problems & Solutions

### "Authentication failed for Docker Hub"

**Problem:** Your secrets are wrong
**Solution:**
1. Verify token is copied correctly (no extra spaces)
2. Regenerate token on Docker Hub if unsure
3. Update secrets in GitHub

### "Dockerfile not found"

**Problem:** Workflow can't find your Dockerfile
**Solution:**
- Ensure `Dockerfile` is in root directory
- Check file is named exactly `Dockerfile` (case-sensitive)

### "Image already exists"

**Problem:** Tag already pushed
**Solution:**
- Change tag: `aivihub-api:v1.1`
- Or overwrite with `latest` tag (default)

### "Permission denied"

**Problem:** Token doesn't have push permission
**Solution:**
1. Regenerate token on Docker Hub
2. Check "Read, Write, Delete" permissions
3. Update GitHub secret

---

## 📊 Workflow Summary

| Step | Time | Action |
|------|------|--------|
| 1 | <1s | Checkout code |
| 2 | <5s | Login Docker |
| 3 | 20-40s | Build image |
| 4 | 10-30s | Push to Docker Hub |
| **Total** | **~2 mins** | **Complete deployment!** |

---

## 🚀 Next Steps

1. ✅ Setup complete
2. Push code to main branch
3. Watch workflow run
4. Check Docker Hub for your image
5. Deploy to AWS EC2 or cloud platform

---

## 📚 Learn More

- [GitHub Actions Docs](https://docs.github.com/en/actions)
- [Docker Hub](https://hub.docker.com/)
- [CI/CD Best Practices](https://docs.github.com/en/actions/guides)

---

**Your CI/CD pipeline is now ready! 🎉**
