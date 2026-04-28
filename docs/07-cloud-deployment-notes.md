# Phase 7: Cloud Deployment Notes

## Azure VM Provisioning
* **OS:** Ubuntu 22.04 LTS
* **Network Security Group (NSG) Hardening:**
    * `Port 80 (HTTP)`: Open (`0.0.0.0/0`) for application serving.
    * `Port 22 (SSH)`: Strictly whitelisted to the administrator's local IP address.
* **Public URL:** `http://102.37.21.215`

## Validations
Deployment to the live cloud environment successfully proved the multi-stage Dockerfiles work outside of a local environment. The Nginx proxy intercepts traffic on the public IP, backend API paths (`/health`) return 200 OK statuses, and Docker Volumes successfully persist data across aggressive `docker-compose down` and `up` cycles.
