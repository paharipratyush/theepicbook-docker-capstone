# Phase 2: Environment Variables & Ports

### Required Environment Variables
Secrets management was handled exclusively via a `.env` file injected at runtime, ensuring zero credential leakage into version control.
* `MYSQL_ROOT_PASSWORD`: Secure root access for DB initialization.
* `MYSQL_DATABASE`, `MYSQL_USER`, `MYSQL_PASSWORD`: Core app credentials.
* `JAWSDB_URL`: The full Sequelize connection string.
* `PORT`: `8080`
* `NODE_ENV`: `production`

### Port Mapping Strategy
* **Nginx:** `80:80` (Exposed to host).
* **Node.js App:** `8080` (Internal).
* **MySQL Database:** `3306` (Internal).

**Learnings:**
Testing `curl -I http://localhost:8080` on the host VM actively failed with a connection refused error, which successfully proved our Docker network isolation was working perfectly. The app is completely shielded from direct host access.
