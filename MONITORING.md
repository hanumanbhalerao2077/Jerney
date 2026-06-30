# MONITORING.md — Prometheus & Grafana

This document explains the monitoring approach for Jerney.

---

## 1) What to monitor

At minimum:
- Backend health (liveness/readiness, HTTP 5xx rate)
- Request latency (p95/p99 if instrumented)
- Throughput (RPS)
- Pod restarts / crash loops

For production, you’d also add:
- DB metrics (connections, query latency)
- container resource usage (CPU/memory)

---

## 2) Prometheus

**Prometheus** collects time-series metrics.

In Kubernetes, you typically deploy Prometheus using:
- `kube-prometheus-stack` Helm chart, or
- a dedicated Prometheus Deployment + Service + scrape configs.

---

## 3) Grafana

**Grafana** displays dashboards.

Best practice:
- Provision a Grafana datasource pointing to Prometheus.
- Provision dashboards via ConfigMaps.

---

## 4) Application health strategy

Even without full custom instrumentation, you can:
- rely on Kubernetes probes for orchestration safety
- track response codes (via ingress/controller metrics)

---

## 5) Interview talking points

- “Prometheus provides the metrics backend; Grafana provides dashboards and alert visualizations.”
- “Liveness/readiness probes prevent traffic to unhealthy pods and aid automated recovery.”

