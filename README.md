# Jerney — Production-Ready DevOps Interview Project

Jerney is a Gen‑Z blog platform built as a **3-tier application**:
- **Frontend**: React (Vite) served by **Nginx**
- **Backend**: Node.js + Express REST API
- **Database**: PostgreSQL

This repository is designed to be **resume-quality** and **DevOps interview-ready** with:
- Docker Compose for local runtime
- Production-minded Dockerfiles
- Full documentation set (architecture, setup, Docker, Kubernetes, CI/CD, security, monitoring)
- Kubernetes + monitoring artifacts (and CI/CD guidance)

---

## Live architecture (summary)

```mermaid
flowchart LR
  U[Users] -->|HTTP| FE[Frontend (Nginx + React)]
  FE -->|/api/*| BE[Backend API (Express)]
  BE -->|SQL| DB[(PostgreSQL)]
```

---

## Quick start (Docker Compose)

1) Install Docker Desktop
2) Run:

```bash
cp .env.example .env 2>nul || true
docker compose up --build
```

Frontend: `http://localhost:8080`

Backend health: `http://localhost:5000/api/health`

---

## Documentation index

- `ARCHITECTURE.md` — Mermaid diagrams + component explanations
- `PROJECT_STRUCTURE.md` — repo layout
- `SETUP.md` — copy/paste setup commands
- `DOCKER.md` — Dockerfiles & compose details
- `KUBERNETES.md` — Kubernetes resource explanations
- `CICD.md` — GitHub Actions pipeline with Trivy scanning
- `SECURITY.md` — security controls and best practices
- `MONITORING.md` — Prometheus/Grafana concepts
- `DEPLOYMENT.md` — local/minikube/k8s/AWS EC2 deployment overview
- `TROUBLESHOOTING.md` — diagnosis playbook
- `INTERVIEW_GUIDE.md` — 2-min/5-min scripts + 50+ DevOps Q/A

