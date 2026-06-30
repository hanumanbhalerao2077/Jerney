# INTERVIEW_GUIDE.md — Jerney DevOps Interview Preparation

This file contains **scripts** and **project-specific interview questions** with detailed answers.

---

## 1) Project explanation (2 minutes)

> Jerney is a Gen‑Z blog platform built with a 3-tier architecture: React frontend served by Nginx, a Node.js/Express backend, and PostgreSQL for persistence. The frontend talks to the backend through `/api/*`, and Nginx proxies API requests to Express. 
>
> For local development and testing, we run the stack with Docker Compose: Postgres, the backend API, and the frontend image. 
>
> For production, we deploy with Kubernetes: Deployments run frontend and backend pods, Services provide stable networking, and Ingress routes external HTTP traffic to the correct service. Postgres uses a PVC for durable storage.
>
> For CI/CD, we use GitHub Actions to build and validate the code, build Docker images, scan them with Trivy, and only deploy if vulnerability thresholds pass. Monitoring is designed around Prometheus and Grafana, and we rely on liveness/readiness probes to ensure safe rollouts.

---

## 2) Project explanation (5 minutes)

Expand with:
- DB schema initialization strategy and why readiness checks matter
- Kubernetes probes (readiness vs liveness)
- ConfigMap/Secret separation
- Supply-chain security: Trivy gating in CI/CD
- Rollbacks via `kubectl rollout undo`
- Observability: Prometheus metrics and Grafana dashboards

---

## 50+ DevOps interview questions with detailed answers

1. **Why use a 3-tier architecture?**
   - Separation of concerns and scaling/failure domains.

2. **How does the frontend reach the backend?**
   - Nginx proxies `/api/` to the Express backend.

3. **What does an Ingress do?**
   - Routes HTTP host/path to internal Services.

4. **Why Kubernetes Deployments?**
   - Desired state, rolling updates, and rollback history.

5. **What does a Kubernetes Service provide?**
   - Stable IP/DNS + load balancing to Pods.

6. **Readiness vs liveness probes?**
   - Readiness gates traffic; liveness restarts unhealthy containers.

7. **What are ConfigMaps and Secrets?**
   - ConfigMaps for non-sensitive values; Secrets for credentials.

8. **Why PVC for Postgres?**
   - Persistent storage for data across Pod restarts/reschedules.

9. **What is Trivy and why scan images?**
   - Detect CVEs in dependencies/base images before deployment.

10. **What happens when Trivy finds critical issues?**
   - CI/CD fails to prevent deploying known-bad artifacts.

11. **How do you remediate vulnerabilities?**
   - Update base image/npm deps, rebuild images, re-scan.

12. **Why multi-stage Docker builds?**
   - Smaller images + reduced attack surface.

13. **Why non-root containers?**
   - Minimizes impact of container compromise.

14. **What is Helmet?**
   - Adds secure HTTP headers to reduce common web attack vectors.

15. **Why rate limiting?**
   - Mitigates brute force and abuse patterns.

16. **How do you secure CORS?**
   - Allow only required origins; avoid wildcard when credentials are involved.

17. **How do you do graceful shutdown?**
   - Stop accepting requests and close server/connection pools during termination.

18. **How do you rollback in Kubernetes?**
   - `kubectl rollout undo deployment/<name>`.

19. **What commands debug a failing pod?**
   - `kubectl logs`, `kubectl describe`, and `kubectl get events`.

20. **How do you validate Kubernetes manifests?**
   - `kubectl apply --dry-run=client -f ...`.

21. **Difference between PV and PVC?**
   - PV is the backing storage; PVC is the claim used by workloads.

22. **How do you handle secrets in CI?**
   - GitHub Secrets; never echo sensitive values.

23. **How do you prevent secrets from leaking in logs?**
   - Avoid logging env vars; mask secrets; restrict access.

24. **How do you choose resource requests/limits?**
   - Based on profiling; ensure requests fit scheduling; limits protect stability.

25. **How do you scale the backend?**
   - Increase replicas; optionally add HPA.

26. **Do you scale Postgres horizontally?**
   - Usually not for basic setups; more often vertical scaling or managed HA.

27. **How do you observe production issues?**
   - Prometheus metrics + Grafana dashboards + logs.

28. **Where do you set monitoring alerts?**
   - In Prometheus alert rules (conceptually), displayed in Grafana.

29. **What is network flow in this app?**
   - Client → Nginx/Ingress → backend service → Postgres.

30. **Why use Services instead of direct Pod IPs?**
   - Pods are ephemeral; Services provide stable endpoints.

31. **How does CI/CD ensure correctness?**
   - Lint/test steps and smoke tests after deployment.

32. **What is a smoke test?**
   - Minimal functional check (e.g., `GET /api/health`).

33. **Why pin GitHub Actions versions?**
   - Supply-chain risk reduction.

34. **What is the image tagging strategy?**
   - Immutable tags like commit SHA for traceability.

35. **What is supply-chain risk?**
   - Vulnerable dependencies/actions compromising builds.

36. **How do you reduce it?**
   - Lock deps, scan images, pin action versions, use signed artifacts.

37. **How do you design readiness for DB-backed services?**
   - Fail readiness until DB is reachable.

38. **How do you handle DB migrations?**
   - Use a job/init migration approach rather than runtime schema creation.

39. **Why should schema creation be controlled?**
   - Avoid unsafe changes during rollouts.

40. **What are common K8s failure modes?**
   - CrashLoopBackOff, Pending pods (PVC/resources), ImagePullBackOff.

41. **How do you resolve CrashLoopBackOff?**
   - Inspect logs, env vars, DB connectivity, permissions.

42. **How do you resolve Pending pods?**
   - Check events, resource requests, PVC binding.

43. **How do you handle Ingress misroutes?**
   - Verify rules/paths/target ports; check ingress controller logs.

44. **How do you protect the API at the edge?**
   - Secure Ingress/Nginx with headers, TLS, and rate limiting.

45. **What is a non-functional requirement?**
   - Security, reliability, performance, observability.

46. **How do you ensure reliability during deploys?**
   - Rolling updates + readiness probes.

47. **How do you ensure performance?**
   - Caching/static hosting + proper resource requests/limits.

48. **How do you ensure data safety?**
   - PVC + backups/managed snapshots (in real production).

49. **What is the role of persistent storage?**
   - Keeps Postgres data across container lifecycle.

50. **What improvements would you propose next?**
   - Separate migration jobs, structured logging, HPA, better metrics.

51. **Explain liveness probe for this app.**
   - Restarts if backend is hung or unresponsive.

52. **Explain readiness probe for this app.**
   - Ensures DB connectivity before serving traffic.


