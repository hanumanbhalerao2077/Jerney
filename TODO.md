# Jerney — Production-Ready DevOps Docs & Infra TODO

## Docs (required)
- [ ] Create `README.md` (complete overview)
- [ ] Create `ARCHITECTURE.md` (Mermaid diagrams + component explanations)
- [ ] Create `PROJECT_STRUCTURE.md`
- [ ] Create `SETUP.md` (step-by-step, copy-paste commands)
- [ ] Create `DOCKER.md`
- [ ] Create `KUBERNETES.md` (namespace, ConfigMap, Secret, Deployment, Service, Ingress, PVC, probes, resources)
- [ ] Create `CICD.md` (GitHub Actions: checkout/build/test/docker/trivy/push/deploy/rollback)
- [ ] Create `SECURITY.md` (helmet, rate limiting, headers, secrets, non-root, read-only FS, trivy scanning)
- [ ] Create `MONITORING.md` (Prometheus + Grafana)
- [ ] Create `DEPLOYMENT.md` (local, docker compose, minikube, k8s, AWS EC2, optional EKS)
- [ ] Create `TROUBLESHOOTING.md`
- [ ] Create `INTERVIEW_GUIDE.md` (2-min/5-min explanations + 50+ Q/A)

## Repo hardening / infra artifacts
- [ ] Add backend hardening: helmet + rate limiting + readiness/health endpoints + graceful shutdown
- [ ] Add GitHub Actions workflows for CI/CD + Trivy scanning
- [ ] Add Kubernetes manifests (Namespace, ConfigMap, Secret, Deployments, Services, Ingress, PVC) with probes/resources
- [ ] Add monitoring stack manifests and documentation (Prometheus/Grafana)

## Validation
- [ ] Ensure `docker compose up --build` works
- [ ] Ensure Docker images build
- [ ] Ensure Kubernetes manifests validate (kubectl apply --dry-run)
- [ ] Ensure Trivy scan command works

## Done
- [✅] Created `TODO.md`
- [ ] Remove placeholders/TODOs from final docs

