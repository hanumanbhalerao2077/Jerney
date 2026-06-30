# Project Structure

This repository is organized as a **production-style, multi-service** project:

```
jerney/
├── backend/                 # Node.js + Express API (PostgreSQL access)
│   ├── src/
│   │   ├── index.js         # Express app bootstrap
│   │   ├── db.js            # PostgreSQL pool + schema bootstrap
│   │   └── routes/
│   │       ├── posts.js    # /api/posts
│   │       └── comments.js # /api/comments
│   ├── Dockerfile
│   └── package.json
│
├── frontend/                # React (Vite) SPA
│   ├── src/
│   ├── Dockerfile
│   ├── nginx.conf          # Nginx to serve SPA + proxy /api
│   └── package.json
│
├── deploy/                  # EC2 (bare-metal) deployment helpers
│   ├── setup.sh
│   └── jerney-nginx.conf
│
├── k8s/                      # Kubernetes manifests
│   └── ...
│
├── monitoring/              # Prometheus/Grafana manifests
│   └── ...
│
├── trivy/                   # Trivy config (optional)
│   └── ...
│
├── .github/workflows/       # GitHub Actions pipelines
│   └── ...
│
├── docker-compose.yml       # Local 3-tier runtime (db + backend + frontend)
└── README.md                 # Root overview
```

## Why this structure
- Separation of concerns (frontend/backed/database).
- Infra artifacts live next to code.
- Easy to explain in interviews.

