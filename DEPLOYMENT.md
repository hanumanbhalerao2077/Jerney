# DEPLOYMENT.md — Step-by-step deployment guide

This guide covers deployment paths for:
- local machine
- Docker Compose
- Minikube
- Kubernetes (generic)
- AWS EC2
- (optional) AWS EKS

---

## 0) Prerequisites

Install:
- Docker Desktop (for local Docker Compose)
- kubectl
- Minikube (for local Kubernetes)

---

## 1) Local Docker Compose

### Start
```bash
cd c:/Users/IK/jerney
cp .env.example .env 2>nul || true
docker compose up --build
```

### Verify
```bash
curl -s http://localhost:5000/api/health
```

### Open frontend
- http://localhost:8080

### Stop
```bash
docker compose down
```

---

## 2) Minikube

### Start minikube
```bash
minikube start --driver=hyperv
```

### Enable ingress
```bash
minikube addons enable ingress
minikube tunnel
```

### Apply manifests
```bash
kubectl apply -f k8s/
```

### Verify
```bash
kubectl get pods -n jerney
kubectl get svc -n jerney
kubectl get ingress -n jerney
```

---

## 3) Kubernetes cluster (generic)

1. Create namespace / secrets / configmaps
2. Apply manifests
3. Verify rollout

```bash
kubectl apply -f k8s/ -n jerney
kubectl rollout status deployment/jerney-backend -n jerney
kubectl rollout status deployment/jerney-frontend -n jerney
```

---

## 4) AWS EC2 (bare metal)

The repo includes `deploy/setup.sh`.

### Steps
```bash
# From your laptop
scp -r ./ ubuntu@<EC2_PUBLIC_IP>:~/jerney

# SSH
ssh ubuntu@<EC2_PUBLIC_IP>

# Setup
cd ~/jerney
chmod +x deploy/setup.sh
./deploy/setup.sh
```

### Access
```text
http://<EC2_PUBLIC_IP>
```

---

## 5) Optional AWS EKS overview

For production-grade EKS:
- Use managed node groups
- Install ingress controller + AWS Load Balancer Controller
- Use EBS-backed StorageClass for Postgres PVC
- Store secrets in Secrets Manager + External Secrets


