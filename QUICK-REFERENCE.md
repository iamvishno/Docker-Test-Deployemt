# ⚡ Quick Reference Guide

## 🚀 Common Commands

### Local Development
```bash
# Install dependencies
pip install -r requirements.txt

# Run server
python main.py

# Access
http://127.0.0.1:8000
```

### Docker
```bash
# Build image
docker build -t iris-ml-app .

# Run container
docker run -p 8000:8000 iris-ml-app

# View logs
docker logs -f <container_id>

# Stop container
docker stop <container_id>

# Remove image
docker rmi iris-ml-app
```

### Docker Compose
```bash
# Build and start
docker-compose up --build

# Run in background
docker-compose up -d

# View logs
docker-compose logs -f

# Stop services
docker-compose down

# Rebuild after code changes
docker-compose up --build
```

### Git & GitHub
```bash
# Add files
git add .

# Commit
git commit -m "Add feature"

# Push to main (triggers CI/CD!)
git push origin main

# Check status
git status
```

### GitHub Actions
```
Repo → Actions → See workflow runs
Click workflow → See build logs
```

### AWS EC2
```bash
# Connect to instance
ssh -i key.pem ec2-user@ip-address

# Pull and run image
docker pull your_username/aivihub-api:latest
docker run -d -p 8000:8000 your_username/aivihub-api:latest

# View running containers
docker ps

# Access app
http://ec2_public_ip:8000
```

---

## 📝 File Overview

| File | When to Edit | Why |
|------|--------------|-----|
| `main.py` | Always | Your ML logic |
| `requirements.txt` | Add libraries | New dependencies |
| `static/index.html` | UI changes | Web interface |
| `static/style.css` | Styling | Colors, fonts, layout |
| `Dockerfile` | Rarely | Container config |
| `docker-compose.yml` | Rarely | Local Docker setup |
| `.github/workflows/build-and-deploy.yml` | Rarely | CI/CD pipeline |

---

## 🔐 Setup Checklist

### GitHub Secrets
```
Required for CI/CD to work:

Repo → Settings → Secrets and variables → Actions

Secret 1:
  Name: DOCKER_USERNAME
  Value: your_docker_username

Secret 2:
  Name: DOCKER_PASSWORD
  Value: your_docker_access_token
```

---

## 🐛 Troubleshooting

### Port 8000 already in use
```bash
# Find what's using port 8000
lsof -i :8000

# Kill process
kill -9 <PID>

# Or change port in docker-compose.yml
ports:
  - "8001:8000"
```

### Docker Hub login failed
```
Check:
1. Username is correct
2. Password is access token (not Docker password)
3. Secrets are set in GitHub
4. Token has "Read, Write, Delete" permissions
```

### Dockerfile not found
```
Check:
1. Dockerfile is in root directory
2. Name is exactly "Dockerfile" (capital D)
3. No file extension
```

### Model not found error
```
Check:
1. scikit-learn installed: pip install scikit-learn
2. numpy installed: pip install numpy
3. No typos in main.py
```

### Slow predictions
```
Solutions:
1. Reduce trees: model = RandomForestClassifier(n_estimators=50)
2. Close background apps
3. Check CPU usage: top (Linux/Mac) or Task Manager (Windows)
```

---

## 📊 GitHub Actions Workflow

```yaml
on:                           # When to trigger
  push:
    branches: [main]          # On push to main

jobs:
  build:                      # Job name
    runs-on: ubuntu-latest    # Where to run

    steps:                    # What to do
      - Checkout code         # 1. Get your code
      - Login to Docker       # 2. Auth with Docker Hub
      - Build image           # 3. docker build
      - Push to Docker        # 4. docker push
```

---

## 🎯 Deployment Flowchart

```
Local Dev
  ↓ (python main.py)
Works?
  ↓ (yes)
Docker Local
  ↓ (docker-compose up)
Works?
  ↓ (yes)
Push to GitHub
  ↓ (git push origin main)
GitHub Actions
  ↓ (automatic build & push)
Docker Hub
  ↓ (image available)
AWS EC2
  ↓ (docker pull & docker run)
Production! ✅
```

---

## 📋 Pre-Deployment Checklist

- [ ] Code tested locally: `python main.py`
- [ ] Works in browser: `http://localhost:8000`
- [ ] Docker build works: `docker build -t iris-ml-app .`
- [ ] Docker run works: `docker run -p 8000:8000 iris-ml-app`
- [ ] `.github/workflows/build-and-deploy.yml` exists
- [ ] GitHub secrets added: `DOCKER_USERNAME`, `DOCKER_PASSWORD`
- [ ] Ready to push to main branch

---

## 🔄 Typical Workflow

```bash
# 1. Make code changes
# 2. Test locally
python main.py
# 3. Test in Docker
docker-compose up
# 4. Git operations
git add .
git commit -m "Feature: add new capability"
git push origin main
# 5. Watch GitHub Actions
# 6. Check Docker Hub for new image
# 7. Deploy to AWS EC2
docker pull your_username/aivihub-api:latest
docker run -d -p 8000:8000 your_username/aivihub-api:latest
```

---

## 📚 Key Concepts

| Term | Meaning |
|------|---------|
| **CI** | Continuous Integration - auto build on push |
| **CD** | Continuous Deployment - auto deploy on build |
| **Container** | Isolated environment with all dependencies |
| **Docker** | Tool to create & run containers |
| **Image** | Blueprint for container (like template) |
| **Registry** | Storage for images (Docker Hub) |
| **Webhook** | GitHub notifies Actions of push |
| **Secret** | Encrypted credential stored in GitHub |
| **Token** | Credential for Docker Hub access |

---

## 🌐 Important URLs

| Service | URL | Purpose |
|---------|-----|---------|
| App (Local) | `http://127.0.0.1:8000` | Development |
| Docker Hub | `https://hub.docker.com` | Image registry |
| GitHub | `https://github.com/your_username/repo` | Code repository |
| GitHub Actions | `https://github.com/your_username/repo/actions` | Workflow runs |
| AWS EC2 | `http://ec2_public_ip:8000` | Production |

---

## 💾 Environment Variables (Optional)

For production, add to docker-compose.yml:
```yaml
environment:
  - ENVIRONMENT=production
  - DEBUG=false
```

In main.py:
```python
import os
debug_mode = os.getenv("DEBUG", "true") == "true"
```

---

## 🔗 API Endpoints

| Method | Endpoint | Purpose | Data |
|--------|----------|---------|------|
| GET | `/` | Get web interface | None |
| POST | `/predict` | Predict iris species | JSON with 4 floats |

---

## 📊 Expected Performance

| Operation | Time | Details |
|-----------|------|---------|
| API call | 100-200ms | Full round trip |
| Model prediction | <50ms | Just inference |
| Frontend render | <1s | Page load |
| Docker build | 20-40s | First time |
| CI/CD pipeline | 2-3 min | Full workflow |

---

## 🆘 Quick Fixes

```bash
# Clear Docker cache and rebuild
docker system prune
docker-compose build --no-cache

# Restart all containers
docker-compose restart

# Kill hanging process
pkill -f uvicorn

# Check if port is open
curl http://localhost:8000

# View Python version
python --version

# Check pip packages
pip list | grep fastapi

# Reinstall all dependencies
pip install --upgrade -r requirements.txt
```

---

## 🎓 Learning Resources

- **FastAPI**: https://fastapi.tiangolo.com/
- **Docker**: https://www.docker.com/
- **GitHub Actions**: https://github.com/features/actions
- **Scikit-learn**: https://scikit-learn.org/
- **Python**: https://www.python.org/

---

## 📞 Quick Help

### GitHub Actions not running?
1. Commit `.github/workflows/build-and-deploy.yml`
2. Push to main branch
3. Check Actions tab

### Image not pushing?
1. Verify secrets in GitHub
2. Test locally: `docker login`
3. Check token permissions

### App won't start?
1. Check logs: `docker logs <container_id>`
2. Test locally: `python main.py`
3. Verify port isn't in use

### Want to change model?
Edit `main.py`:
```python
# Line ~30: Change model
model = RandomForestClassifier(n_estimators=50)  # Change from 100

# Or use different algorithm
from sklearn.ensemble import GradientBoostingClassifier
model = GradientBoostingClassifier()
```

---

**Bookmark this page for quick reference! 📌**
