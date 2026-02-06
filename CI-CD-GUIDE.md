# 🚀 GitHub Actions CI/CD Pipeline - Complete Guide

## Overview

This GitHub Actions workflow automates the process of building and deploying your Iris ML application to Docker Hub whenever you push code to the main branch.

## What is CI/CD?

**CI/CD** stands for **Continuous Integration / Continuous Deployment**

- **CI (Continuous Integration)**: Automatically test and build code when changes are pushed
- **CD (Continuous Deployment)**: Automatically deploy the built application to production

## Workflow Breakdown

### 📋 Trigger Configuration

```yaml
on:
  push:
    branches: [main]
```

**What it does:**
- Listens for push events to the `main` branch
- Automatically starts the workflow whenever code is pushed to main
- Other branches won't trigger this workflow

**Other trigger options:**
```yaml
# Trigger on pull requests
on:
  pull_request:
    branches: [main]

# Trigger on schedule (e.g., daily at 2 AM UTC)
on:
  schedule:
    - cron: '0 2 * * *'

# Trigger manually
on:
  workflow_dispatch:
```

---

### 🖥️ Job Configuration

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
```

**What it means:**
- `build`: Name of the job
- `runs-on: ubuntu-latest`: Runs on latest Ubuntu virtual machine provided by GitHub
- GitHub provides free runners with: 2 CPU, 7GB RAM, 14GB SSD

**Other runner options:**
- `ubuntu-20.04` - Specific Ubuntu version
- `windows-latest` - Windows environment
- `macos-latest` - macOS environment
- `self-hosted` - Your own machine

---

### ⚙️ Step-by-Step Execution

#### **Step 1: Checkout Code**

```yaml
- name: Checkout code
  uses: actions/checkout@v3
```

**What it does:**
- Downloads your GitHub repository code to the runner
- Makes all your files available for the build process
- `@v3` specifies the version of the action

**Why it's needed:**
- Workflow needs access to your source code
- Needs `Dockerfile` to build the image
- Accesses `.github/workflows/` config files

---

#### **Step 2: Login to Docker Hub**

```yaml
- name: Login to Docker Hub
  uses: docker/login-action@v2
  with:
    username: ${{ secrets.DOCKER_USERNAME }}
    password: ${{ secrets.DOCKER_PASSWORD }}
```

**What it does:**
- Authenticates with Docker Hub registry
- Uses secrets stored in GitHub for credentials
- Creates authenticated session for pushing images

**Security Note:**
- `${{ secrets.DOCKER_USERNAME }}` retrieves secret from GitHub
- Secrets are encrypted and never shown in logs
- `uses: docker/login-action@v2` is official Docker action

**How to setup secrets:**
1. Go to GitHub repo → Settings → Secrets and variables → Actions
2. Click "New repository secret"
3. Add two secrets:
   - `DOCKER_USERNAME`: Your Docker Hub username
   - `DOCKER_PASSWORD`: Your Docker Hub access token

**Example:**
```
Name: DOCKER_USERNAME
Secret: your_docker_username

Name: DOCKER_PASSWORD
Secret: dckr_pat_xxxxxxxxxxxx
```

⚠️ **Never hardcode credentials!** Always use secrets.

---

#### **Step 3: Build Docker Image**

```yaml
- name: Build Docker Image
  run: docker build -t ${{ secrets.DOCKER_USERNAME }}/aivihub-api:latest .
```

**What it does:**
- Builds Docker image using your `Dockerfile`
- Tags image as `username/aivihub-api:latest`
- `.` specifies build context (current directory)

**Command breakdown:**
```bash
docker build                          # Build command
  -t owner/repo-name:tag             # Tag with repository name
  ${{ secrets.DOCKER_USERNAME }}     # Docker Hub username
  /aivihub-api:latest                # Image name and tag
  .                                   # Current directory (Dockerfile location)
```

**Image naming convention:**
```
docker_username/image_name:tag
     ↓                    ↓    ↓
  your_user    /iris-ml-app:1.0
```

**Tag options:**
- `latest` - Newest version (default)
- `v1.0`, `v2.0` - Semantic versioning
- `main`, `develop` - Branch-based tags

---

#### **Step 4: Push to Docker Hub**

```yaml
- name: Push to Docker Hub
  run: docker push ${{ secrets.DOCKER_USERNAME }}/aivihub-api:latest
```

**What it does:**
- Uploads built image to Docker Hub registry
- Makes image publicly available
- Other developers can pull your image

**Command breakdown:**
```bash
docker push username/image-name:tag
# Uploads to: https://hub.docker.com/r/username/image-name
```

---

## 🔄 Complete Workflow Execution

### Timeline of Events:

```
1. Developer pushes code to main branch
                    ↓
2. GitHub detects push → Triggers workflow
                    ↓
3. GitHub creates Ubuntu runner instance
                    ↓
4. STEP 1: Checkout code (downloads your repo)
                    ↓
5. STEP 2: Login to Docker (authenticate with Docker Hub)
                    ↓
6. STEP 3: Build image (runs: docker build -t ...)
                    ↓
7. STEP 4: Push image (runs: docker push ...)
                    ↓
8. Workflow completes ✅ or fails ❌
                    ↓
9. GitHub sends notification (email/Slack)
```

---

## 📊 Workflow Visualization

```
┌─────────────────────────────────────────────────────────┐
│          GitHub Repository (Your Code)                   │
│  ┌──────────────────────────────────────────────────┐   │
│  │  main branch                                      │   │
│  │  ├── main.py                                     │   │
│  │  ├── requirements.txt                            │   │
│  │  ├── Dockerfile                                  │   │
│  │  └── static/                                     │   │
│  └──────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
                         │
                    You push code
                         │
                         ↓
    ╔═════════════════════════════════════════════════╗
    ║    GitHub Actions Workflow Triggered            ║
    ║  (build-and-deploy.yml executed)                ║
    ╚═════════════════════════════════════════════════╝
                         │
        ┌────────────────┼────────────────┐
        ↓                ↓                ↓
    ┌────────┐    ┌────────────┐   ┌──────────┐
    │Checkout│    │Login Docker│   │Build Image│
    │  Code  │───→│   Hub      │──→│from Docker│
    └────────┘    └────────────┘   │  file    │
                                    └──────────┘
                                        │
                                        ↓
                                    ┌──────────┐
                                    │Push Image│
                                    │to Docker │
                                    │  Hub     │
                                    └──────────┘
                                        │
                    ┌───────────────────┴───────────────────┐
                    ↓                                       ↓
            ✅ Success                            ❌ Failed
            Image in Docker Hub                   Notification
```

---

## 🔐 Setup Instructions

### 1. Create GitHub Secrets

1. Go to your repository on GitHub
2. Click **Settings** → **Secrets and variables** → **Actions**
3. Click **New repository secret**
4. Add these secrets:

| Secret Name | Value | Where to get |
|---|---|---|
| `DOCKER_USERNAME` | Your Docker Hub username | https://hub.docker.com/settings/security |
| `DOCKER_PASSWORD` | Docker Hub Access Token | https://hub.docker.com/settings/security |

### 2. Create Docker Hub Access Token

1. Log in to Docker Hub
2. Go to Account Settings → Security → Personal access tokens
3. Click **Create access token**
4. Name it: `github-actions`
5. Copy the token (you won't see it again!)
6. Paste in GitHub secret as `DOCKER_PASSWORD`

### 3. Commit Workflow File

```bash
git add .github/workflows/build-and-deploy.yml
git commit -m "Add GitHub Actions CI/CD workflow"
git push origin main
```

---

## 📈 Monitoring & Logs

### View Workflow Status

1. Go to your GitHub repo
2. Click **Actions** tab
3. See all workflow runs
4. Click on a run to see details

### View Logs

Click on a job to see step-by-step logs:
- ✅ Green checkmark = Step passed
- ❌ Red X = Step failed
- 🟡 Yellow = Step in progress

### Common Issues & Fixes

| Issue | Solution |
|-------|----------|
| `Login failed` | Verify Docker Hub username/token in secrets |
| `Dockerfile not found` | Ensure `Dockerfile` is in repo root |
| `docker push failed` | Check token permissions in Docker Hub |
| `Permission denied` | Verify GitHub Actions has access to secrets |

---

## 🚀 Advanced Configuration

### Add Testing Before Build

```yaml
- name: Run Tests
  run: |
    pip install -r requirements.txt
    pytest tests/
```

### Build Multiple Tags

```yaml
- name: Build Docker Image
  run: |
    docker build -t ${{ secrets.DOCKER_USERNAME }}/aivihub-api:latest .
    docker build -t ${{ secrets.DOCKER_USERNAME }}/aivihub-api:${{ github.sha }} .
```

### Trigger on Version Tags

```yaml
on:
  push:
    tags:
      - 'v*.*.*'
```

### Add Slack Notifications

```yaml
- name: Notify Slack
  uses: slackapi/slack-github-action@v1.24.0
  with:
    webhook-url: ${{ secrets.SLACK_WEBHOOK }}
    payload: |
      {
        "text": "Deployment complete! 🚀"
      }
```

---

## 📊 Workflow Status Badge

Add this to your README.md:

```markdown
![Build and Deploy](https://github.com/yourusername/repo-name/actions/workflows/build-and-deploy.yml/badge.svg)
```

---

## 💰 GitHub Actions Pricing

- **Free tier**: 2,000 minutes/month (unlimited for public repos)
- **1 workflow run** ≈ 1-5 minutes
- Build + push typically takes 2-3 minutes

---

## 🔄 Full CI/CD Pipeline Architecture

```
┌──────────────────────────────────────────────────────────┐
│                  Developer's Local Machine               │
│  ┌────────────────────────────────────────────────────┐ │
│  │  $ git push origin main                            │ │
│  └────────────────────────────────────────────────────┘ │
└──────────────────────────────────────────────────────────┘
                         │
                         ↓
┌──────────────────────────────────────────────────────────┐
│                   GitHub Repository                      │
│  ┌────────────────────────────────────────────────────┐ │
│  │  .github/workflows/build-and-deploy.yml (executed) │ │
│  └────────────────────────────────────────────────────┘ │
└──────────────────────────────────────────────────────────┘
                         │
                         ↓
┌──────────────────────────────────────────────────────────┐
│              GitHub Actions (CI Pipeline)                │
│  ┌────────────────────────────────────────────────────┐ │
│  │  1. Checkout code                                  │ │
│  │  2. Login to Docker Hub                            │ │
│  │  3. Build Docker image                             │ │
│  │  4. Push to Docker Hub                             │ │
│  └────────────────────────────────────────────────────┘ │
└──────────────────────────────────────────────────────────┘
                         │
                         ↓
┌──────────────────────────────────────────────────────────┐
│               Docker Hub Registry (CD)                    │
│  ┌────────────────────────────────────────────────────┐ │
│  │  yourusername/aivihub-api:latest                  │ │
│  │  - Ready for deployment                            │ │
│  │  - Accessible from anywhere                        │ │
│  └────────────────────────────────────────────────────┘ │
└──────────────────────────────────────────────────────────┘
                         │
                         ↓
        ┌────────────────┴────────────────┐
        │                                 │
        ↓                                 ↓
   ┌─────────────┐              ┌──────────────────┐
   │ AWS EC2     │              │ Kubernetes Cluster│
   │ (Production)│              │ (Staging)        │
   │             │              │                  │
   │docker pull  │              │docker pull       │
   │docker run   │              │docker run        │
   └─────────────┘              └──────────────────┘
```

---

## 🎯 Benefits of This CI/CD Pipeline

✅ **Automation**: No manual steps - push code and it deploys
✅ **Consistency**: Same build process every time
✅ **Speed**: Automatic builds take 2-3 minutes
✅ **Reliability**: Catches errors early
✅ **Scalability**: Works with any number of developers
✅ **Security**: Credentials stored safely in secrets
✅ **Visibility**: Easy to track deployment history

---

## 📝 Summary

| Component | Purpose |
|-----------|---------|
| **Trigger** | `push to main` - Detects code changes |
| **Runner** | `ubuntu-latest` - Where workflow runs |
| **Checkout** | Get code from repository |
| **Login** | Authenticate with Docker Hub |
| **Build** | Create Docker image from Dockerfile |
| **Push** | Upload image to Docker Hub |

---

## 🔗 Useful Links

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Docker Login Action](https://github.com/docker/login-action)
- [Docker Hub](https://hub.docker.com/)
- [GitHub Secrets](https://docs.github.com/en/actions/security-guides/encrypted-secrets)

---

**Your CI/CD pipeline is ready to automate your deployments! 🚀**
