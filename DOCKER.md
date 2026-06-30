# DOCKER.md — Dockerfiles & Docker Compose

## Images vs Containers
- **Image**: immutable artifact built from Dockerfile.
- **Container**: running instance of an image.

---

## Backend Dockerfile (`backend/Dockerfile`)

This file uses **multi-stage builds**.

### Build stage
- Installs dependencies using `npm ci`.

```dockerfile
FROM node:20-alpine AS build
WORKDIR /app
COPY package.json package-lock.json* ./
RUN npm ci --only=production && npm cache clean --force
```

### Production stage
- Creates a non-root user.
- Copies only runtime artifacts.
- Uses `dumb-init`.

```dockerfile
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
RUN apk --no-cache add dumb-init
...
USER appuser
ENTRYPOINT ["dumb-init", "--"]
CMD ["node", "src/index.js"]
```

Why this is production-ready:
- smaller runtime footprint (no build tooling)
- non-root user
- correct signal handling (PID 1)

---

## Frontend Dockerfile (`frontend/Dockerfile`)

Multi-stage build:
1) build React app into `dist`
2) serve via `nginx:alpine`

Also uses `frontend/nginx.conf`:
- SPA fallback to `index.html`
- proxies `/api/` to backend
- security headers

---

## Docker Compose (`docker-compose.yml`)

Three services:
- `db`: PostgreSQL 16
- `backend`: builds from `./backend`, connects to `db`
- `frontend`: builds from `./frontend`, exposed on host `8080`

Volumes:
- `jerney-postgres-data` persists Postgres data.

### Common commands

Build:
```bash
docker compose build --no-cache
```

Run:
```bash
docker compose up
```

Logs:
```bash
docker compose logs -f
```

Teardown:
```bash
docker compose down
```

Teardown + delete data:
```bash
docker compose down -v
```

