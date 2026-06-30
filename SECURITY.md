# SECURITY.md — Security & Hardening

This document covers the security measures used and how to explain them in an interview.

---

## 1) Helmet (security headers)

**Helmet** adds secure HTTP headers to reduce browser-side attack vectors.

Common headers:
- `X-Frame-Options`
- `X-Content-Type-Options`
- `Content-Security-Policy`
- `Referrer-Policy`

**Why it matters**: reduces impact of XSS/clickjacking/mime-sniffing.

> Note: Add Helmet in backend application code.

---

## 2) Rate limiting

Add **rate limiting** to protect endpoints from abuse.

**Why it matters**:
- mitigates brute force and denial-of-service patterns
- protects the database from request storms

---

## 3) Secure headers at the edge (Nginx)

The frontend Nginx configuration includes:
- secure headers
- dotfile denial

**Why**: enforce baseline security at the ingress point.

---

## 4) Secrets management

- Docker Compose/local: use environment variables (acceptable for dev).
- Kubernetes: use **ConfigMap** for non-sensitive values and **Secret** for credentials.

**Why**: prevents accidental credential leakage.

---

## 5) Non-root containers

- backend image runs as a non-root user (`appuser`)
- frontend Nginx runs as non-root (`nginx`)

**Why**: reduces privilege impact if the app is compromised.

---

## 6) Read-only filesystem (where possible)

In Kubernetes, you can enable:
- `readOnlyRootFilesystem: true`
- mount writeable volumes only where required

**Why**: reduces persistence and attacker surface.

---

## 7) Image scanning using Trivy

Use **Trivy** in CI/CD:
- scan built images
- gate deployments on severity thresholds (fail on `CRITICAL`/`HIGH`)

**Why**: prevents deploying known-vulnerable dependencies.

---

## 8) Best practices checklist

- pin dependency versions with lockfiles
- use multi-stage Docker builds to reduce image size
- pin GitHub Actions versions
- do not store secrets in git
- use readiness/liveness probes to avoid sending traffic to unhealthy pods

---

## Interview-ready phrasing

“Jerney applies defense-in-depth across CI/CD, runtime hardening, and edge security. We run non-root containers, enforce secure headers, add rate limiting and Helmet, store credentials in Kubernetes Secrets, and use Trivy scanning as a deployment gate.”

