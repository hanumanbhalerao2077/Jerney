# KUBERNETES.md — Kubernetes deployment resources (Jerney)

This document explains each Kubernetes resource used to run Jerney in a cluster.

> This repository will also include concrete manifest examples under `k8s/`.

---

## 1) Namespace

- **What**: A namespace groups related resources.
- **Why**:
  - isolate dev/stage/prod
  - apply RBAC, resource quotas, and network policies per environment.

---

## 2) ConfigMap

- **What**: Key/value store for **non-sensitive** configuration.
- **Used for**:
  - app settings like log level, feature flags, DB host name.

Why it exists:
- avoids baking environment values into container images.

---

## 3) Secret

- **What**: Key/value store for **sensitive** configuration.
- **Used for**:
  - `DB_USER`, `DB_PASSWORD`, and other credentials.

Why it exists:
- prevents secrets from being committed to git.

---

## 4) Deployment

- **What**: Declares desired state for a set of Pods.
- **Used for**:
  - backend API
  - frontend (Nginx)

Key fields to know:
- `replicas`: number of desired Pods
- `strategy`: rolling updates behavior
- `resources.requests` / `resources.limits`

---

## 5) ReplicaSet

- **What**: The controller Deployment manages.
- **Why**:
  - keeps the number of Pods matching your desired state.

---

## 6) Pod

- **What**: Runs the container(s).
- **Why**:
  - holds runtime state, has probes, env vars, volumes.

---

## 7) Service

- **What**: Stable network identity + load balancing.
- **Used for**:
  - exposing backend to other services
  - exposing frontend internally so Ingress can route to it.

---

## 8) Ingress

- **What**: HTTP routing entry point.
- **Used for**:
  - routing external requests to frontend and backend paths.

In this app, you typically route:
- `/` → frontend service
- `/api/*` → backend service

---

## 9) Persistent Volume (PV) and Persistent Volume Claim (PVC)

- **PV**: actual storage resource in the cluster.
- **PVC**: claim requested by the workload.

Why it matters:
- PostgreSQL must keep data across Pod restarts and node reschedules.

---

## 10) Resource Requests and Limits

- **requests**: scheduler-guaranteed minimum
- **limits**: hard cap enforced by the runtime

Why:
- improves bin packing, avoids noisy-neighbor issues.

---

## 11) Readiness Probe

- **What**: Pod is ready to receive traffic.
- **For Jerney backend**:
  - check DB connectivity (or a dedicated readiness endpoint)

Why:
- prevents sending requests to an unhealthy Pod.

---

## 12) Liveness Probe

- **What**: Pod is alive; restart if it’s stuck.

Why:
- recovers from deadlocks or runtime hangs.

---

## What you should be able to explain in an interview
- How ConfigMap/Secret map to environment variables
- Why Services are stable while Pods are ephemeral
- Why readiness is critical for dependency-backed apps (DB)
- Why PVC is required for Postgres durability

