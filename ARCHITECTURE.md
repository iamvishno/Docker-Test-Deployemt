# 🏗️ Complete Architecture - ML App with CI/CD

## System Overview

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                          Complete Architecture                               │
└─────────────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────┐
│                     DEVELOPMENT PHASE                            │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │  Developer's Local Machine                                │ │
│  │  ├── Edit code (main.py, index.html, etc.)               │ │
│  │  ├── Test locally: python main.py                        │ │
│  │  ├── Run tests                                            │ │
│  │  └── Git push to main branch                             │ │
│  └────────────────────────────────────────────────────────────┘ │
└──────────────────────────────────────────────────────────────────┘
                              │
                         git push
                              │
                              ↓
┌──────────────────────────────────────────────────────────────────┐
│                  GITHUB (VERSION CONTROL)                        │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │  Repository: docker-ml-app-ci-cd-hub-aws-ec2             │ │
│  │  Branch: main                                             │ │
│  │  Files:                                                   │ │
│  │  ├── main.py (FastAPI app)                               │ │
│  │  ├── requirements.txt (dependencies)                      │ │
│  │  ├── Dockerfile (container config)                        │ │
│  │  ├── static/index.html (frontend)                         │ │
│  │  ├── static/style.css (styling)                           │ │
│  │  └── .github/workflows/build-and-deploy.yml (CI/CD)      │ │
│  └────────────────────────────────────────────────────────────┘ │
└──────────────────────────────────────────────────────────────────┘
                              │
                    Webhook trigger
                              │
                              ↓
┌──────────────────────────────────────────────────────────────────┐
│             GITHUB ACTIONS (CI/CD AUTOMATION)                    │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │  Workflow: "Build and Deploy"                             │ │
│  │                                                            │ │
│  │  Jobs:                                                     │ │
│  │  ┌──────────────────────────────────────────────────────┐│ │
│  │  │ Step 1: Checkout Code                               ││ │
│  │  │ ✓ Downloads repository                              ││ │
│  │  │ ✓ Access to all files                               ││ │
│  │  └──────────────────────────────────────────────────────┘│ │
│  │           │                                               │ │
│  │           ↓                                               │ │
│  │  ┌──────────────────────────────────────────────────────┐│ │
│  │  │ Step 2: Login to Docker Hub                          ││ │
│  │  │ ✓ Read secrets from GitHub                           ││ │
│  │  │ ✓ DOCKER_USERNAME: your_username                    ││ │
│  │  │ ✓ DOCKER_PASSWORD: your_access_token                ││ │
│  │  │ ✓ Authenticate with registry                         ││ │
│  │  └──────────────────────────────────────────────────────┘│ │
│  │           │                                               │ │
│  │           ↓                                               │ │
│  │  ┌──────────────────────────────────────────────────────┐│ │
│  │  │ Step 3: Build Docker Image                           ││ │
│  │  │ Command: docker build -t user/aivihub-api:latest .  ││ │
│  │  │ ✓ Read Dockerfile                                   ││ │
│  │  │ ✓ Install dependencies                              ││ │
│  │  │ ✓ Create container image                            ││ │
│  │  └──────────────────────────────────────────────────────┘│ │
│  │           │                                               │ │
│  │           ↓                                               │ │
│  │  ┌──────────────────────────────────────────────────────┐│ │
│  │  │ Step 4: Push to Docker Hub                           ││ │
│  │  │ Command: docker push user/aivihub-api:latest         ││ │
│  │  │ ✓ Upload image to registry                           ││ │
│  │  │ ✓ Image available globally                           ││ │
│  │  └──────────────────────────────────────────────────────┘│ │
│  │           │                                               │ │
│  │           ↓                                               │ │
│  │  ✅ Workflow Complete                                    │ │
│  │  Notification sent to developer                          │ │
│  └────────────────────────────────────────────────────────────┘ │
└──────────────────────────────────────────────────────────────────┘
                              │
                     Docker image pushed
                              │
                              ↓
┌──────────────────────────────────────────────────────────────────┐
│           DOCKER HUB (CONTAINER REGISTRY)                        │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │  Repository: your_username/aivihub-api                   │ │
│  │  Tags:                                                     │ │
│  │  ├── latest (most recent)                                │ │
│  │  ├── v1.0 (version)                                      │ │
│  │  └── main (branch)                                       │ │
│  │                                                            │ │
│  │  Image Details:                                            │ │
│  │  ├── Size: ~500-800 MB (Python:3.11-slim)               │ │
│  │  ├── Base: Ubuntu Linux                                   │ │
│  │  ├── Python: 3.11                                        │ │
│  │  ├── Entrypoint: uvicorn main:app                       │ │
│  │  └── Accessible: docker pull your_username/aivihub-api │ │
│  └────────────────────────────────────────────────────────────┘ │
└──────────────────────────────────────────────────────────────────┘
                              │
              Image ready for deployment
                              │
              ┌───────────────┼───────────────┐
              ↓               ↓               ↓
    ┌──────────────┐  ┌──────────────┐  ┌──────────────┐
    │  AWS EC2     │  │ Kubernetes   │  │  Docker      │
    │ (Production) │  │ (Staging)    │  │  Swarm       │
    └──────────────┘  └──────────────┘  └──────────────┘
           │               │                    │
           ↓               ↓                    ↓
    Pull & Run:    Pull & Run:         Pull & Run:
    docker pull     kubectl apply      docker service
    docker run      -f deploy.yaml     create --image
```

---

## Component Interactions

### 1️⃣ Local Development → GitHub Push

```
Your Computer
├── Edit: main.py
├── Edit: requirements.txt
├── Test: python main.py
├── Test: browser http://localhost:8000
└── Deploy: git push origin main
        ↓
    GitHub receives push
        ↓
    Webhook fires (GitHub Actions)
```

### 2️⃣ GitHub Actions Workflow

```
Trigger: push to main branch
    ↓
Queue Job: build (runs-on: ubuntu-latest)
    ↓
Create Runner: Spawn Ubuntu VM
    ↓
Execute Steps:
    1. actions/checkout@v3 → Get your code
    2. docker/login-action@v2 → Auth with Docker Hub
    3. docker build -t ... → Build image from Dockerfile
    4. docker push ... → Push to registry
    ↓
Cleanup: Destroy VM
    ↓
Notify: Email/Slack with status
```

### 3️⃣ Docker Image Creation

```
Dockerfile
    ↓
FROM python:3.11-slim (Start with slim Python)
    ↓
WORKDIR /app (Create /app directory)
    ↓
COPY requirements.txt . (Copy dependencies list)
    ↓
RUN pip install (Install all packages)
    ↓
COPY . . (Copy application code)
    ↓
EXPOSE 8000 (Port configuration)
    ↓
CMD uvicorn ... (Run server)
    ↓
Build Layer by Layer (Cached for speed)
    ↓
Result: your_username/aivihub-api:latest
    (500-800 MB image with everything needed to run)
```

### 4️⃣ Deployment to AWS EC2

```
AWS EC2 Instance
├── OS: Ubuntu/Amazon Linux
├── Docker: Pre-installed
└── Deploy Script:
    ├── aws ecr get-login (if using AWS ECR)
    ├── docker pull your_username/aivihub-api:latest
    ├── docker run -d \
    │   -p 8000:8000 \
    │   -e ENVIRONMENT=prod \
    │   your_username/aivihub-api:latest
    └── Open port 8000 in Security Group
        ↓
    Application running on:
    http://your_ec2_public_ip:8000
```

---

## Data Flow

```
User Browser
    ↓
http://localhost:8000
    ↓
FastAPI (main.py)
├── GET / → Serve index.html
├── POST /predict → Process prediction
    ├── Extract features from JSON
    ├── Scale features (StandardScaler)
    ├── Predict (RandomForestClassifier)
    ├── Get probabilities
    └── Return JSON response
    ↓
JavaScript (index.html)
    ├── Fetch POST to /predict
    ├── Parse JSON response
    ├── Update HTML with results
    └── Show progress bars
    ↓
User sees: Prediction + Confidence Scores
```

---

## File Structure

```
docker-ml-app-ci-cd-hub-aws-ec2/
│
├── main.py                          # FastAPI application
│   ├── Load Iris dataset
│   ├── Train RandomForest model
│   ├── Define /predict endpoint
│   └── Serve static files
│
├── requirements.txt                 # Python dependencies
│   ├── fastapi==0.104.1
│   ├── uvicorn==0.24.0
│   ├── scikit-learn==1.3.2
│   ├── numpy==1.24.3
│   └── pydantic==2.4.2
│
├── Dockerfile                       # Container definition
│   ├── Base image: python:3.11-slim
│   ├── Install dependencies
│   ├── Copy code
│   └── Run uvicorn
│
├── docker-compose.yml               # Local container orchestration
│   ├── Service: ml-app
│   ├── Port: 8000:8000
│   ├── Volume: ./static
│   └── Restart policy
│
├── .dockerignore                    # Build optimization
│   └── Exclude: cache, venv, git files
│
├── static/                          # Frontend files
│   ├── index.html                   # Web interface
│   │   ├── Form inputs
│   │   ├── JavaScript (fetch + DOM)
│   │   └── Result display
│   │
│   └── style.css                    # Styling
│       ├── Gradients
│       ├── Animations
│       └── Responsive design
│
├── .github/                         # GitHub configuration
│   └── workflows/
│       └── build-and-deploy.yml     # CI/CD workflow
│           ├── Trigger: push to main
│           ├── Job: build
│           ├── Step: checkout
│           ├── Step: login docker
│           ├── Step: build image
│           └── Step: push to hub
│
├── README.md                        # Documentation
│   ├── Features
│   ├── Setup instructions
│   ├── API endpoints
│   └── Performance notes
│
├── CI-CD-GUIDE.md                   # Detailed explanation
│   ├── CI/CD concepts
│   ├── Workflow breakdown
│   ├── Setup instructions
│   └── Advanced configs
│
├── GITHUB-ACTIONS-SETUP.md          # Quick start guide
│   ├── 5-minute setup
│   ├── Docker Hub secrets
│   ├── Workflow monitoring
│   └── Troubleshooting
│
└── ARCHITECTURE.md                  # This file
    ├── System overview
    ├── Component interactions
    ├── Data flow
    └── Deployment options
```

---

## Technology Stack

| Layer | Technology | Purpose |
|-------|-----------|---------|
| **Language** | Python 3.11 | Application logic |
| **Web Framework** | FastAPI | REST API server |
| **ASGI Server** | Uvicorn | Production server |
| **ML Library** | Scikit-learn | Iris classification model |
| **Frontend** | HTML5 + CSS3 + JS | Web interface |
| **Container** | Docker | Containerization |
| **Registry** | Docker Hub | Image storage |
| **CI/CD** | GitHub Actions | Automation |
| **VCS** | Git/GitHub | Version control |
| **Cloud** | AWS EC2 | Deployment target |

---

## Deployment Scenarios

### Scenario 1: Local Development
```
python main.py
→ http://localhost:8000
```

### Scenario 2: Local Container
```
docker build -t iris-app .
docker run -p 8000:8000 iris-app
→ http://localhost:8000
```

### Scenario 3: Docker Compose
```
docker-compose up --build
→ http://localhost:8000
```

### Scenario 4: Production (AWS EC2)
```
1. Push code to GitHub main branch
2. GitHub Actions builds & pushes to Docker Hub
3. SSH into EC2 instance
4. docker pull your_username/aivihub-api:latest
5. docker run -d -p 8000:8000 your_username/aivihub-api:latest
→ http://ec2_public_ip:8000
```

### Scenario 5: Kubernetes
```
1. Image in Docker Hub
2. kubectl create deployment iris-app \
   --image=your_username/aivihub-api:latest
3. kubectl expose deployment iris-app \
   --type=LoadBalancer --port=8000
→ Distributed across cluster
```

---

## Security Considerations

```
┌─────────────────────────────────────────────┐
│          Security Layers                    │
└─────────────────────────────────────────────┘

1. GitHub Secrets (Encrypted)
   └─ DOCKER_USERNAME
   └─ DOCKER_PASSWORD
   └─ Never exposed in logs

2. Docker Hub Access Token
   └─ Limited to "Read, Write, Delete"
   └─ Can be revoked anytime

3. EC2 Security Groups
   └─ Only port 8000 exposed
   └─ SSH only from trusted IPs

4. Container Security
   └─ Non-root user execution
   └─ Read-only filesystem possible
   └─ Resource limits (CPU, Memory)
```

---

## Performance Metrics

| Component | Performance |
|-----------|-------------|
| **CI/CD Pipeline** | ~2-3 minutes |
| **Model Inference** | <50ms per prediction |
| **API Response Time** | ~100-200ms |
| **Frontend Load Time** | <1s |
| **Docker Image Build** | 20-40s (first build) |
| **Docker Image Size** | 500-800 MB |

---

## Monitoring & Logging

```
GitHub Actions Logs
    ↓ Shows build/push status

Docker Hub Registry
    ↓ Shows image versions

Application Logs (in container)
    └─ uvicorn startup messages
    └─ Request logs
    └─ Error traces

AWS CloudWatch (Optional)
    └─ Container logs
    └─ Performance metrics
    └─ Alerts
```

---

## Summary

✅ **Development**: Edit code locally, test, push to GitHub
✅ **CI (Continuous Integration)**: GitHub Actions builds & tests
✅ **CD (Continuous Deployment)**: Automatically pushed to Docker Hub
✅ **Deployment**: Pull from Docker Hub to production

**Result**: One `git push` → Automatic deployment! 🚀
