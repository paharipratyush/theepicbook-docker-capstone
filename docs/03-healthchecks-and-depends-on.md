# Phase 3: Healthchecks & Startup Order

### The Problem: Race Conditions
In distributed systems, bringing all containers up simultaneously causes crashes. If the Node.js app boots before MySQL finishes initializing its connection pools, the app throws an `ECONNREFUSED` error and dies.

### The Solution: Cascading Startup Order
We implemented strict healthchecks combined with `depends_on: condition: service_healthy` to force an orderly boot sequence.
1.  **MySQL:** Polled via `mysqladmin ping`.
2.  **Node.js App:** Will not start until MySQL is healthy. Polled via `wget --spider http://localhost:8080/health`.
3.  **Nginx:** Will not start until Node.js is healthy.

**Implementation Snippet:**
```yaml
    depends_on:
      mysql:
        condition: service_healthy
    healthcheck:
      test: ["CMD", "wget", "--no-verbose", "--tries=1", "--spider", "http://localhost:8080/health"]
      interval: 10s
      timeout: 5s
      retries: 5
```
