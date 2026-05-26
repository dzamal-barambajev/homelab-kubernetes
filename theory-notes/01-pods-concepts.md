# Kubernetes Pods: Kernkonzepte

Ein **Pod** ist die kleinste bereitstellbare (deploybare) Recheneinheit, die in Kubernetes erstellt und verwaltet werden kann.

### 📋 Hauptmerkmale
- **Gemeinsames Netzwerk (Shared Network):** Alle Container innerhalb eines einzelnen Pods teilen sich denselben Netzwerk-Namespace, dieselbe IP-Adresse und denselben Port-Bereich (sie können direkt über `localhost` miteinander kommunizieren).
- **Gemeinsamer Speicher (Shared Storage):** Ein Pod kann eine Reihe von gemeinsamen Speicherки-Volumes definieren, auf die alle Container im Pod Zugriff haben.
- **Flüchtige Natur (Ephemeral Nature):** Pods sind standardmäßig als temporäre und austauschbare Einheiten konzipiert. Wenn unvorhergesehen ein Pod ausfällt, wird er von Kubernetes durch einen neuen Pod ersetzt, anstatt den alten zu reparieren.

### 🎯 Mein Lernziel
Verstehen, wie Single- und Multi-Container-Pods mithilfe von YAML-Manifesten definiert und über das CLI-Tool `kubectl` effizient verwaltet werden.

---

### 🚀 Lab 01: Zusammenfassung des ersten Pod-Deployments

* **Pod-Name**: `my-first-nginx`
* **Status**: Erfolgreich aktiv (`1/1 Running`)
* **Zugewiesene Cluster-IP**: `10.42.0.5`
* **Verifizierung**: Ein `curl http://10.42.0.5` wurde direkt vom Host-System aus ausgeführt. Die Anfrage lieferte erfolgreich den HTTP-Status `200 OK` zusammen mit der Nginx-Willkommensseite zurück.
* **Protokollierung (Logs)**: Überprüft mit `kubectl logs my-first-nginx`. Die Logs bestätigten einen sauberen Container-Start und erfassten die eingehende `curl`-Anfrage fehlerfrei.
