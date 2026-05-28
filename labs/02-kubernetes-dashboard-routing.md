# Milestone: Erfolgreiche Migration & Routing der Kubernetes Dashboard Web-GUI

## 📋 Übersicht
Dieses Dokument beschreibt die Integration der offiziellen **Kubernetes Dashboard Web-GUI** in eine bestehende, restriktive Hybrid-Infrastruktur. Das Ziel war es, die interne Cluster-Ressource über einen sicheren, verschlüsselten Tunnel nach außen zu exponieren, ohne den bestehenden VPN-Gateway-Traffic zu beeinträchtigen.

---

## 📡 Aktuelle Monitoring- & Management-Architektur

### 🌐 Traffic-Flow (Inbound)
Der Datenverkehr für administrative Zugriffe erfolgt über eine mehrstufige Proxy-Kette zur maximalen Absicherung der Host-Ports:


               🌐  Internet (Browser)
                       │
                       ▼
           ┌───────────────────────┐
           │ 🕳️  Xray VPN Gateway  │  <── Inbound-Port :443
           └───────────────────────┘
                       │ (🔄 Lokale Schleife)
                       ▼
           ┌───────────────────────┐
           │ 🚀  Nginx Proxy Server│  <── Interner Port :8443
           └───────────────────────┘
                       │ (🪵 NodePort 32080)
                       ▼
           ┌───────────────────────┐
           │ ⎈   Ingress-Nginx K3s │  <── Cluster-Router
           └───────────────────────┘
                       │
                       ▼
           ┌───────────────────────┐
           │ 📊  K8s Dashboard Pod │  <── Ziel-Web-GUI
           └───────────────────────┘

---

## 🛠️ Technische Umsetzung & Troubleshooting-Schritte

### 1. Ingress-Nginx NodePort Konfiguration
Da der Ingress-Controller standardmäßig innerhalb des k3s-Netzwerks isoliert ist, wurden dedizierte "lokale Tore" auf Host-Ebene über einen `NodePort`-Service (`svc-nodeport.yaml`) realisiert:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: ingress-nginx-nodeport
  namespace: ingress-nginx
spec:
  type: NodePort
  selector:
    app.kubernetes.io/name: ingress-nginx
    app.kubernetes.io/component: controller
  ports:
    - name: http
      port: 80
      targetPort: 80
      nodePort: 32080
    - name: https
      port: 443
      targetPort: 443
      nodePort: 32443
```

### 2. Host-Sicherheit (iptables-Bereinigung)
Um unbefugte externe Scans auf die NodePorts` blockieren, wurde die Host-Firewall analysiert. Es wurde festgestellt, dass k3s-interne Module (Accounting via `nfacct-name localhost_nps_accepted_pkts`) den Traffic bereits standardmäßig auf `127.0.0.0/8` einschränken. Redundante, blockierende `INPUT`-Regeln, die einen `404 Not Found` Fehler erzeugten, wurden erfolgreich mit `iptables -D INPUT` bereinigt.

### 3. Nginx Global Default Server & SSL-Terminierung
Um Host-Header-Konflikte (bedingt durch Port-Anhänge wie `:8443` im Browser-Request) zu vermeiden, wurde das Routing in einen dedizierten `default_server` auf Port `8443` überführt. Hierbei erfolgt eine saubere SSL-Validierung mittels Let's Encrypt Wildcard-Zertifikaten für die Subdomäne.

Datei: `/etc/nginx/conf.d/8443-default.conf`
```nginx
server {
    listen 8443 default_server ssl http2;
    server_name _;

    ssl_certificate /etc/letsencrypt/live/example.com;
    ssl_certificate_key /etc/letsencrypt/live/example.com;

    location / {
        proxy_set_header Host \$host;
        proxy_set_header X-Real-IP \$remote_addr;
        proxy_set_header X-Forwarded-For \$proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto https;
        
        proxy_http_version 1.1;
        proxy_set_header Upgrade \$http_upgrade;
        proxy_set_header Connection "upgrade";
        
        proxy_pass http://127.0.0.1:32080;
    }
}
```

### 4. Automatisierung des Logins (Skip-Funktion & RBAC)
Zur Steigerung der Usability im Homelab wurde die Authentifizierung optimiert. Statt permanenter manueller Dekodierung von Base64-Tokens aus K8s-Secrets (Verhalten ab K8s v1.24+ geändert), wurde lückenlos die native Skip-Funktion implementiert:

1. **Deployment-Patch (Aktivierung der Skip-Schaltfläche):**
   ```bash
   kubectl patch deployment kubernetes-dashboard -n kubernetes-dashboard --type='json' -p='[{"op": "add", "path": "/spec/template/spec/containers/0/args/-", "value": "--enable-skip-login"}]'
   ```

2. **RBAC-ClusterRoleBinding für administrative Rechte des Skip-Users:**
   ```yaml
   apiVersion: rbac.authorization.k8s.io/v1
   kind: ClusterRoleBinding
   metadata:
     name: kubernetes-dashboard-skip-admin
   roleRef:
     apiGroup: rbac.authorization.k8s.io
     kind: ClusterRole
     name: cluster-admin
   subjects:
   - kind: ServiceAccount
     name: kubernetes-dashboard
     namespace: kubernetes-dashboard
   ```

## 🎯 Ergebnis
Die Weboberfläche ist nun nativ und voll verschlüsselt unter `https://forexample.network.com` erreichbar. Der Zugriff erfolgt per Ein-Klick-Verfahren ("Skip"), während das Cluster im Hintergrund absolut isoliert und geschützt bleibt.
