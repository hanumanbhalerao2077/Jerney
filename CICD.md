# CICD.md — GitHub Actions CI/CD with Trivy

This pipeline is designed to be interview-ready:
- build & lint/test
- build Docker images
- scan with Trivy
- push images
- deploy to Kubernetes
- support rollback

---

## 1) Recommended workflow file

Create `.github/workflows/ci-cd.yml`.

---

## 2) Pipeline stages explanation

### Checkout
`actions/checkout` pulls your code.

### Build + Test
- Backend: `npm ci` + `npm run lint`
- Frontend: `npm ci` + `npm run build`

### Docker build
Builds backend and frontend images.

### Trivy scan
Trivy scans each image.
If vulnerabilities exceed thresholds (example: `CRITICAL`), the pipeline fails.

### Push images
Push images to a registry (GHCR/Docker Hub/ECR).

### Deploy
Deploy to Kubernetes via:
- `kubectl apply` (manifests), or
- Helm (if using charts).

### Rollback
Use:
- `kubectl rollout undo deployment/<name>`

---

## 3) Example pipeline (template)

Create the workflow with this template (adjust registry/deploy parts):

```yaml
name: CI-CD

on:
  push:
    branches: [ main ]
  workflow_dispatch:

jobs:
  build-test-scan-deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: 20

      - name: Backend install + lint
        working-directory: backend
        run: |
          npm ci
          npm run lint

      - name: Frontend install + build
        working-directory: frontend
        run: |
          npm ci
          npm run build

      - name: Build Docker images
        run: |
          docker build -t jerney-backend:${{ github.sha }} ./backend
          docker build -t jerney-frontend:${{ github.sha }} ./frontend

      - name: Trivy scan backend
        uses: aquasecurity/trivy-action@0.20.0
        with:
          image-ref: jerney-backend:${{ github.sha }}
          format: table
          severity: CRITICAL,HIGH
          exit-code: 1

      - name: Trivy scan frontend
        uses: aquasecurity/trivy-action@0.20.0
        with:
          image-ref: jerney-frontend:${{ github.sha }}
          format: table
          severity: CRITICAL,HIGH
          exit-code: 1

      - name: Deploy to Kubernetes
        env:
          KUBE_CONFIG: ${{ secrets.KUBE_CONFIG }}
        run: |
          echo "$KUBE_CONFIG" > kubeconfig
          export KUBECONFIG=./kubeconfig
          kubectl apply -f k8s/
```

