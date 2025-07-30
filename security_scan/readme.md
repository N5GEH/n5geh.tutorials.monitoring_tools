# Analysis of the security of services

## Scan of the Docker images
- Use of [Trivy](https://trivy.dev/latest/) as a popular open source security scanner is effective.
- Connection possible via container registry
    - Possible, for example, in [Harbor](https://goharbor.io/)
    - Scanner is installed here as a container and can automatically check all new images --> Option to display vulnerabilities assigned to the images
    - Similar option in Docker Hub
- Use via CLI -- Connection to monitoring services such as CheckMK or Centreon conceivable
    - Local scan can be performed
        - Install Trivy: https://trivy.dev/v0.18.3/installation/
        - Scan all images: `docker ps -q | xargs -I {} sh -c "trivy image \$(docker inspect --format='{{index .Config.Image}}' {})"`
- Use with Ansible Playbook (can be executed via CI tool)
    - Execution of the Ansible playbook [`playbook_trivy.yml`](playbook_trivy.yml) is possible, but a section with `[trivy]` must be created in `Hosts.ini`:
        - Example:
            ```
            [trivy]
            localhost ansible_connection=local
            server2 ansible_host=$IP ansible_user=$USER
            ```
        - The Ansible user ($USER) needs access to the server.
    - The script determines all images on the system, scans them and stores the results under `./trivy/results`
    - The path to the Trivy cache can be set with the environment variable `TRIVY_CACHE_DIR`, otherwise `./trivy/cache`
    - Automatic scanning with Gitlab CI is possible, see [trivy-scan-gitlab-ci.yml](trivy-scan-gitlab-ci.yml) section `run_ansible_trivy_scan`.
        - The results are stored as Artifacts
        - A recurring scan can be set up via the Gitlab UI (`Build` - `Pipeline schedules`).


## Other possibilities
- Not considered here
