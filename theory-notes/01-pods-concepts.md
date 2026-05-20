# Kubernetes Pods: Core Concepts

A **Pod** is the smallest deployable unit of computing that you can create and manage in Kubernetes.

### Key Characteristics
- **Shared Network:** All containers within a single Pod share the same network namespace, IP address, and port space (they can communicate via `localhost`).
- **Shared Storage:** A Pod can specify a set of shared storage volumes that all containers in the Pod can access.
- **Ephemeral Nature:** Pods are disposable and temporary by design. If a Pod dies, Kubernetes replaces it with a new one rather than repairing it.

### My Learning Objective
Understand how to define single and multi-container Pods using YAML manifests and manage them via `kubectl`.
