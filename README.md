# Homelab Kubernetes

![Kubernetes](https://img.shields.io/badge/Kubernetes-k3s-blue?logo=kubernetes)
![Docker](https://img.shields.io/badge/Docker-Containers-blue?logo=docker)
![Grafana](https://img.shields.io/badge/Grafana-Monitoring-orange?logo=grafana)
![Prometheus](https://img.shields.io/badge/Prometheus-Metrics-orange?logo=prometheus)
![Linux](https://img.shields.io/badge/Linux-Ubuntu-black?logo=linux)
![Nginx](https://img.shields.io/badge/Nginx-Reverse_Proxy-green?logo=nginx)

---

## 📋 Übersicht

Dieses Repository dokumentiert meinen Weg bei der Migration einer Self-Hosted-Infrastruktur von eigenständigen Docker-Workloads zu einer leichtgewichtigen Kubernetes-Umgebung (k3s).

Das Ziel ist es, praktische Erfahrung in folgenden Bereichen zu sammeln:

- Kubernetes-Grundlagen
- Container-Orchestrierung
- Infrastruktur-Monitoring
- Ingress-Networking
- Observability (Überwachung)
- Automatisierungs-Workflows
- Produktionsnahes Infrastruktur-Management

---

## 🛠️ Aktuelle Infrastruktur

Der bestehende Self-Hosted-Stack umfasst bereits:

- Ubuntu VPS
- Docker
- Nginx
- Grafana
- Prometheus
- Uptime Kuma
- Xray Core
- Ansible

---

## ⎈ Geplanter Kubernetes-Stack


| Komponente | Zweck |
|---|---|
| k3s | Leichtgewichtiges Kubernetes |
| ingress-nginx | Reverse Proxy und Ingress-Routing |
| Prometheus | Metrik-Erfassung |
| Grafana | Dashboards und Visualisierung |
| Persistent Volumes | Persistenter Speicher für Stateful-Dienste |
| Deployments | Workload-Orchestrierung |
| Services | Internes Netzwerk-Routing |

---

## ⏳ Migrationsfortschritt

- [x] Repository-Initialisierung
- [x] Infrastruktur-Planung
- [ ] Bereinigung der Docker-Workloads
- [x] K3s-Installation
- [ ] Erstes Kubernetes-Deployment
- [ ] Ingress-Konfiguration
- [x] Migration des Monitorings
- [ ] Einrichtung von persistentem Speicher
- [ ] CI/CD-Experimente

---

## 📂 Repository-Struktur

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

## 🎯 Aktueller Fokus

- Kubernetes-Grundlagen
- Infrastruktur-Organisation
- Monitoring und Automatisierung
- Reverse-Proxy-Networking
- Praxisnahe DevOps-Workflows
- Infrastruktur-Observability
- Container-Orchestrierung

---

## 📝 Notizen

Dieses Repository repräsentiert ein sich ständig weiterentwickelndes Homelab und eine Lernumgebung, einschließlich Experimenten, Fehlerbehebungen (Troubleshooting), Migrationsschritten und Infrastruktur-Verbesserungen.


---

## 📑 Meilenstein: Erfolgreiche Migration von Uptime Kuma

**Status:** Aktiv im K3s-Cluster (`uptime-kuma-deployment` läuft stabil)

Um die Migration transparent zu dokumentieren, wurden folgende Schritte durchgeführt:

1. **Infrastruktur-as-Code:** Das Manifest `k8s/deployments/uptime-kuma.yaml` wurde erstellt. Es kapselt ein `Deployment` (1 Replica), einen `Service` (festgelegt auf NodePort `32001`) und einen `PersistentVolumeClaim` (2Gi über den K3s `local-path` Provider) [🔍].
2. **Stateful-Datenmigration:** 
   * Der K3s-Replica-Satz wurde temporär auf `0` herunterskaliert, um Schreibzugriffe zu blockieren.
   * Die bestehende SQLite-Datenbank (`kuma.db`) wurde mit dem Befehl `cp -a` unter Beibehaltung aller Berechtigungen aus dem alten Docker-Volume in das persistente K3s-Verzeichnis (`/var/lib/rancher/k3s/storage/`) migriert [🔍].
   * Nach dem Upscaling auf `1` wurden alle historischen Statistiken, Monitore und Benutzerdaten erfolgreich im Cluster wiederhergestellt [🔍].
3. **Nginx-Routing & Bereinigung:** 
   * Der externe Reverse Proxy wurde so konfiguriert, dass er HTTP-Traffic intern an den NodePort `32001` weiterleitet.
   * Der alte, redundante Docker-Container `uptime-kuma` wurde gestoppt und vollständig vom Server entfernt, um RAM-Ressourcen freizugeben.
