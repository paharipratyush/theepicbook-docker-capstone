# Phase 9: Reliability Tests

Intentional chaos testing was performed to validate the resilience of the containerized architecture.

**Test 1: Frontend Proxy Resilience (Backend Failure)**
* **Action:** Executed `docker stop epicbook-app` to simulate a Node.js fatal crash.
* **Result:** **PASS**. Nginx remained online and correctly served a `502 Bad Gateway` error instead of a hanging browser connection. Once the app container was restarted, Nginx resumed routing seamlessly.

**Test 2: Downstream Dependency Failure (DB Outage)**
* **Action:** Executed `docker stop epicbook-mysql` to simulate a database outage.
* **Result:** **PASS**. The Node.js application began throwing `ConnectionRefused` errors and its `/health` endpoint degraded. `docker-compose ps` confirmed the App container successfully transitioned from `(healthy)` to an `unhealthy` state, proving our monitoring setup accurately reflects downstream failures.

**Stretch Goal: Container Resource Limits**
To prevent a compromised container from taking down the Azure VM, hard resource limits were applied to the Node.js application in `docker-compose.yml` (`cpus: 0.50`, `memory: 512M`). Execution of `docker stats` verified Docker was actively enforcing the 512MiB ceiling.
