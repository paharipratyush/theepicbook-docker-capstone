# Phase 6: Logging & Observability

### Log Storage Strategy
* **App Logs:** Captured via Docker's native daemon (`stdout`).
* **Nginx Logs:** Bind-mounted directly to the Azure VM host (`./logs/nginx:/var/log/nginx`). This allows operators to run `tail -f access.log` without needing to exec into the proxy container.

### Structured JSON Implementation
Standard Nginx logs are plain text, making them difficult for observability tools (like ELK or Datadog) to parse. We completely rewrote the Nginx `log_format` to emit `json_combined`.

**Proof of Output:**
```json
{"time_local":"28/Apr/2026:19:51:36 +0000", "remote_addr":"172.19.0.1", "request":"GET / HTTP/1.1", "status": "200", "body_bytes_sent":"24858"}
```
