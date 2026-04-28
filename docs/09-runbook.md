# Phase 9: Ops Runbook

### Infrastructure Operations
* **Start Stack:** `docker-compose up -d`
* **Graceful Stop:** `docker-compose stop`
* **Full Teardown:** `docker-compose down`

### Deployment Rollback Procedure
If a pipeline deployment introduces a critical bug:
1. Locate the last stable Git SHA tag in Azure Container Registry / Docker Hub.
2. Edit `docker-compose.yml` to target the stable image: `image: <registry>/epicbook-app:<STABLE_SHA>`
3. Run: `docker-compose up -d app`

### Backup & Restore
* **Backup:** `docker exec epicbook-mysql /usr/bin/mysqldump -u root -prootpassword123 bookstore > backup.sql`
* **Restore:** `cat backup.sql | docker exec -i epicbook-mysql mysql -u root -prootpassword123 bookstore`

### Troubleshooting Known Errors
* **Error:** `Failed to connect to localhost port 8080` when testing from the host.
    * **Fix:** This is intended behavior. The application is isolated on the backend Docker network. Test against port `80` (Nginx) instead.
* **Error:** CI/CD Pipeline fails with `KeyError: 'ContainerConfig'`.
    * **Fix:** Clear the zombie container via `docker-compose down`. Ensure pipeline uses strict `stop` and `rm` steps before `up`.
