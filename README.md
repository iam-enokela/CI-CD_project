# 🐳 Dockerized Flask + Redis App with CI/CD Pipeline

![CI/CD Pipeline](https://img.shields.io/badge/CI%2FCD-GitHub%20Actions-blue?logo=github-actions)
![Docker](https://img.shields.io/badge/Docker-Containerized-2496ED?logo=docker)
![Flask](https://img.shields.io/badge/Flask-Python-000000?logo=flask)
![Redis](https://img.shields.io/badge/Redis-In--Memory%20Store-DC382D?logo=redis)

A production-style multi-container web application built with Flask and Redis, fully containerized with Docker and deployed via an automated CI/CD pipeline using GitHub Actions.

Every push to the `main` branch automatically builds, tests, and publishes a fresh Docker image to Docker Hub — no manual steps required.

---

## 📐 Architecture

```
Browser Request
      │
      ▼
┌─────────────────┐        ┌─────────────────┐
│   Flask App     │◄──────►│     Redis       │
│   (Port 5000)   │        │   (Port 6379)   │
└─────────────────┘        └─────────────────┘
        │
        │  Docker Compose manages both containers
        │  on a shared internal network
        ▼
  Visit counter persisted
  across requests in Redis
```

```
CI/CD Flow:

Push to GitHub
      │
      ▼
GitHub Actions Runner (Ubuntu)
      │
      ├── Build Docker image
      │
      ├── Spin up containers & run health check (curl)
      │
      └── Push image to Docker Hub ✓
```

---

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| Python / Flask | Web application framework |
| Redis | In-memory data store for visit counter |
| Docker | Containerization |
| Docker Compose | Multi-container orchestration |
| GitHub Actions | CI/CD pipeline automation |
| Docker Hub | Container image registry |

---

## 📁 Project Structure

```
.
├── app.py                        # Flask application
├── requirements.txt              # Python dependencies
├── Dockerfile                    # Container build instructions
├── docker-compose.yml            # Multi-container configuration
└── .github/
    └── workflows/
        └── ci.yml            # CI/CD pipeline definition
```

---

## 🚀 Running Locally

### Prerequisites
- [Docker Desktop](https://www.docker.com/products/docker-desktop/) installed and running

### Steps

**1. Clone the repository**
```bash
git clone https://github.com/yourusername/your-repo-name.git
cd your-repo-name
```

**2. Start the application**
```bash
docker compose up
```

**3. Open in browser**
```
http://127.0.0.1:8080
```

You should see the Flask app running with a live visit counter powered by Redis.

**4. Stop the application**
```bash
docker compose down
```

---

## ⚙️ CI/CD Pipeline

The GitHub Actions workflow (`.github/workflows/ci.yml`) runs automatically on every push to `main`.

### Pipeline Steps

```
1. Checkout code
2. Build Docker image
3. Start containers with Docker Compose
4. Wait for app to be ready (sleep 10s)
5. Run health check with curl
6. Log in to Docker Hub using repository secrets
7. Push image to Docker Hub
```

### Setting Up the Pipeline (for your own fork)

You need to add two secrets to your GitHub repository:

**Settings → Secrets and variables → Actions → New repository secret**

| Secret | Value |
|--------|-------|
| `DOCKERHUB_USERNAME` | Your Docker Hub username |
| `DOCKERHUB_TOKEN` | Docker Hub Access Token (Read & Write) |

To create a Docker Hub token: **hub.docker.com → Account Settings → Security → New Access Token**

---

## 🔑 Key Concepts Demonstrated

**Docker**
- Writing a `Dockerfile` with a slim Python base image
- Building and tagging images
- Port mapping (`-p host:container`)
- Multi-container orchestration with Docker Compose
- Inter-container networking (Flask reaching Redis by service name)
- Volume-less in-memory persistence with Redis

**CI/CD**
- Automated pipeline triggered on push to `main`
- Health check testing before publishing
- Secure credential handling with GitHub Secrets
- Conditional steps (only push on `main`, not on pull requests)
- Publishing to a public container registry

---

## 📦 Docker Hub

The latest image is automatically published on every successful build:

```bash
docker pull enokela12/myapp:latest
docker run -p 8080:5000 enokela12/myapp:latest
```

---

## 📝 What I Learned

- How Docker containers isolate application environments and solve dependency conflicts
- Why `host='0.0.0.0'` is required for Flask to be reachable inside a Docker container
- How Docker Compose creates an internal network where services communicate by name
- The difference between a Docker image (blueprint) and a container (running instance)
- How to securely store credentials in GitHub Actions using repository secrets
- How CI/CD pipelines automate the build → test → deploy cycle

---

## 🗺️ Next Steps

- [ ] Add pytest unit tests to the pipeline
- [ ] Deploy to Kubernetes with a Deployment, Service, and Ingress
- [ ] Add Nginx as a reverse proxy layer
- [ ] Swap Redis for PostgreSQL with persistent volumes
- [ ] Add Prometheus + Grafana monitoring

---

## 👤 Author

**Enokela**
- GitHub: [@iam-enokela](https://github.com/iam-enokela)
- Docker Hub: [enokela12](https://hub.docker.com/u/enokela12)
