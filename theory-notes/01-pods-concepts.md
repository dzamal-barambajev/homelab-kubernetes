# Kubernetes Pods: Core Concepts

A **Pod** is the smallest deployable unit of computing that you can create and manage in Kubernetes.

### Key Characteristics
- **Shared Network:** All containers within a single Pod share the same network namespace, IP address, and port space (they can communicate via `localhost`).
- **Shared Storage:** A Pod can specify a set of shared storage volumes that all containers in the Pod can access.
- **Ephemeral Nature:** Pods are disposable and temporary by design. If a Pod dies, Kubernetes replaces it with a new one rather than repairing it.

### My Learning Objective
Understand how to define single and multi-container Pods using YAML manifests and manage them via `kubectl`.

### 🚀 Lab 01: First Pod Deployment Summary

* **Pod Name**: `my-first-nginx`
* **Status**: Successfully running (`1/1 Running`)
* **Allocated Cluster IP**: `10.42.0.5`
* **Verification**: Executed `curl http://10.42.0.5` from the host machine. Successfully received HTTP `200 OK` with the Nginx welcome page.
* **Logs**: Inspected using `kubectl logs my-first-nginx`, which confirmed the container started cleanly and captured the inbound `curl` request.
