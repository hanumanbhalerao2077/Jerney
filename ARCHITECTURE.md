# ARCHITECTURE.md — Architecture & Diagrams (Mermaid)

Jerney uses a layered architecture with clear separation of concerns:
- **Frontend**: React SPA served by Nginx
- **Backend**: Node.js/Express API
- **Database**: PostgreSQL

---

## 1) High-Level Architecture Diagram

```mermaid
flowchart LR
  U[Users / Browsers] -->|HTTP| FE[Frontend (React + Nginx)]
  FE -->|/api/*| BE[Backend API (Node.js/Express)]
  BE -->|SQL| DB[(PostgreSQL)]
```

### Why each component is used
- **Frontend**: fast UI delivery, independent scaling, SPA UX.
- **Backend**: encapsulates business logic + validation and provides stable API.
- **PostgreSQL**: relational data model for posts/comments, supports integrity constraints.

---

## 2) Docker Architecture

```mermaid
flowchart TB
  DB[postgres:16-alpine] --> BE[jerney-backend]
  BE --> FE[jerney-frontend (nginx)]
```

---

## 3) Kubernetes Architecture

```mermaid
flowchart LR
  subgraph K8s[Cluster]
    IN[Ingress] --> FS[Frontend Service]
    FS --> FP[Frontend Pod]

    IN --> BS[Backend Service]
    BS --> BP[Backend Pod]

    BP --> PG[(Postgres via PVC)]
  end
```

---

## 4) CI/CD Pipeline Diagram

```mermaid
flowchart TD
  A[GitHub Push] --> B[Checkout]
  B --> C[Build + Lint/Test]
  C --> D[Docker Build]
  D --> E[Trivy Scan]
  E --> F[Push Images]
  F --> G[Deploy to Kubernetes]
  G --> H{Smoke Test}
  H -- success --> I[Healthy Release]
  H -- fail --> J[Rollback]
```

---

## 5) Network Flow Diagram

```mermaid
flowchart LR
  Client[Client] -->|GET /| Nginx[Nginx (frontend)]
  Client -->|GET /api/*| Nginx
  Nginx -->|proxy_pass| Backend[Express API]
  Backend --> Postgres[(Postgres)]
```

---

## 6) Request Flow Diagram

```mermaid
sequenceDiagram
  participant C as Client
  participant N as Nginx
  participant A as API
  participant P as Postgres

  C->>N: GET /api/posts
  N->>A: Proxy /api/posts
  A->>P: SELECT posts + comment_count
  P-->>A: rows
  A-->>N: JSON posts
  N-->>C: JSON posts
```

---

## 7) Monitoring Architecture

```mermaid
flowchart LR
  Backend[Backend + Nginx] --> PR[Prometheus]
  PR --> GD[Grafana Dashboards]
```

---

## 8) Security Flow

```mermaid
flowchart TD
  CI[CI Build] --> TR[Trivy Scan]
  TR --> G{Critical vulns?}
  G -- yes --> X[Fail pipeline]
  G -- no --> D[Deploy]

  Requests[Ingress requests] --> H[Secure headers + rate limiting]
  H --> API[Backend API]
```

---

## Component explanations (quick)
- **Frontend**: React UI, served behind Nginx.
- **Backend**: Express routes with PostgreSQL queries.
- **PostgreSQL**: durable storage for posts/comments.
- **Docker**: consistent packaging for local and CI.
- **Kubernetes**: scalable orchestration (Deployments, Services, Ingress).
- **Ingress**: external routing.
- **ConfigMap**: non-sensitive config.
- **Secret**: sensitive credentials.
- **Persistent Volume**: underlying storage.
- **Services**: stable networking.
- **Pods**: runtime units.
- **Deployments**: desired state and rollouts.
- **GitHub Actions**: CI/CD automation.
- **Trivy**: vulnerability scanning.
- **Prometheus/Grafana**: monitoring.

