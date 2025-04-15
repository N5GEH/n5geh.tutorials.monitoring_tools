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
    - Prüfen / TODO