# Analyse der Sicherheit genutzter Services
## Scan der genutzten Docker Images
- Nutzung von [Trivy](https://trivy.dev/latest/) als beliebter open source Sicherheitsscanner
- Anbindung möglich über Container Registry
    - bspw. [Harbor](https://goharbor.io/)
    - Scanner wird hier als Container installiert und kann automatisiert alles neuen Images prüfen --> Möglichkeit der Darstellung der Schwachstellen zugeordnet zu den Images
- Nutzung über CLI -- Anbindung an CheckMK oder Centreon
    - lokaler Scan kann erfolgen
        - Trivy installieren:
            - https://trivy.dev/v0.18.3/installation/
        - Alle Images scannen:
            - `docker ps -q | xargs -I {} sh -c "trivy image \$(docker inspect --format='{{index .Config.Image}}' {})"`
- Nutzung mit Ansible Playbook (Ausführung über CI-Tool möglich)
    - Ausführung des Ansible Playbooks [`playbook_trivy.yml`](playbook_trivy.yml) möglich, muss in der Hosts.ini ein Abschnitt mits `[trivy]` angelegt werden
    - Skript ermittelt alle Images auf dem System und scannt diese und legt die Ergebnisse unter `./trivy/results` ab
    - Pfad zum Cache von Trivy kann mit dern Umgebungsvaribale `TRIVY_CACHE_DIR` gesetzt werden, sonst `./trivy/cache`
    - automatischer Scan mit der Gitlab-CI möglich, siehe [.gitlab-ci.yml](./../.gitlab-ci.yml) Abschnit `run_ansible_trivy_scan`
