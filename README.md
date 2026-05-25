# Homelab Kubernetes

![Kubernetes](https://img.shields.io/badge/Kubernetes-k3s-blue?logo=kubernetes)
![Docker](https://img.shields.io/badge/Docker-Containers-blue?logo=docker)
![Grafana](https://img.shields.io/badge/Grafana-Monitoring-orange?logo=grafana)
![Prometheus](https://img.shields.io/badge/Prometheus-Metrics-orange?logo=prometheus)
![Linux](https://img.shields.io/badge/Linux-Ubuntu-black?logo=linux)
![Nginx](https://img.shields.io/badge/Nginx-Reverse_Proxy-green?logo=nginx)

---

## Overview

This repository documents my journey of migrating a self-hosted infrastructure
from standalone Docker workloads to a lightweight Kubernetes (k3s) environment.

The goal is to gain practical experience with:

- Kubernetes fundamentals
- Container orchestration
- Infrastructure monitoring
- Ingress networking
- Observability
- Automation workflows
- Production-style infrastructure management

---

## Current Infrastructure

Existing self-hosted stack already includes:

- Ubuntu VPS
- Docker
- Nginx
- Grafana
- Prometheus
- Uptime Kuma
- Xray Core
- Ansible

---

## Planned Kubernetes Stack

| Component | Purpose |
|---|---|
| k3s | Lightweight Kubernetes |
| ingress-nginx | Reverse proxy and ingress |
| Prometheus | Metrics collection |
| Grafana | Dashboards and observability |
| Persistent Volumes | Stateful storage |
| Deployments | Workload orchestration |
| Services | Internal networking |

---

## Migration Progress

- [x] Repository initialization
- [x] Infrastructure planning
- [ ] Docker workload cleanup
- [x] Install k3s
- [ ] First Kubernetes deployment
- [ ] Configure ingress
- [ ] Monitoring migration
- [ ] Persistent storage setup
- [ ] CI/CD experiments

---

## Repository Structure

```text
.
├── docs/
├── diagrams/
├── screenshots/
├── scripts/
└── k8s/
    ├── deployments/
    ├── services/
    ├── ingress/
    ├── monitoring/
    └── storage/
```

---

## Current Focus

- Kubernetes fundamentals
- Infrastructure organization
- Monitoring and automation
- Reverse proxy networking
- Real-world DevOps workflows
- Infrastructure observability
- Container orchestration

---

## Notes

This repository represents an evolving homelab and learning environment,
including experiments, troubleshooting, migration steps and infrastructure improvements.
