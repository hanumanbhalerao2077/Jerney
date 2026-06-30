# TROUBLESHOOTING.md — Common failures & fixes

This playbook is written for Docker Compose and Kubernetes.

---

## 1) Docker container not starting

### Verify logs
```bash
docker compose ps

docker compose logs -f backend

docker compose logs -f frontend

docker compose logs -f db
```

### Common causes
- wrong DB credentials
- missing env vars
- backend port mismatch

---

## 2) PostgreSQL connection failed (backend)

### Check DB is reachable
```bash
docker compose exec -T db psql -U "$DB_USER" -d "$DB_NAME" -c "SELECT 1"
```

### Check backend env values
- `DB_HOST=db`
- `DB_PORT=5432`

---

## 3) Image build fails

### Backend
```bash
docker compose build backend --no-cache
```

### Frontend
```bash
docker compose build frontend --no-cache
```

---

## 4) Kubernetes: CrashLoopBackOff

### Inspect logs
```bash
kubectl logs -n jerney <pod> --previous
kubectl describe pod -n jerney <pod>
```

Common causes for this app:
- missing DB env vars
- DB user/password incorrect
- schema init fails due to permissions

---

## 5) Kubernetes: Pending Pods

### Inspect scheduling
```bash
kubectl describe pod -n jerney <pod>
```

Common causes:
- PVC not bound
- insufficient resources

---

## 6) PVC issues

```bash
kubectl get pvc -n jerney
kubectl describe pvc -n jerney <pvc>
```

Common causes:
- StorageClass misconfiguration
- no provisioner in cluster

---

## 7) Service unreachable

```bash
kubectl get svc -n jerney
kubectl get endpoints -n jerney
```

---

## 8) Ingress issues

```bash
kubectl describe ingress -n jerney
kubectl logs -n ingress-nginx <ingress-nginx-controller-pod>
```

---

## 9) Trivy failures in CI

Re-run locally:

```bash
trivy image --severity HIGH,CRITICAL jerney-backend:TAG
```

Fix by:
- updating base images
- updating npm dependencies
- rebuilding

---

## 10) GitHub Actions failures

Checklist:
- required secrets exist in repo settings
  - `KUBE_CONFIG` for kubectl deploy
  - registry credentials (if pushing images)
- Trivy threshold settings match expectations

