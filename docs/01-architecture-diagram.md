# Phase 1: App Discovery & Architecture

**Architecture Overview:**
The EpicBook application operates as a monolithic Node.js service (handling both EJS frontend views and backend API routes) connected to a MySQL database. To harden this for production, the architecture was containerized and placed behind an Nginx reverse proxy.

**Component Flow:**
`User/Internet (Port 80) -> Nginx Reverse Proxy -> Node.js App (Port 8080) -> MySQL Database (Port 3306)`

**Network Isolation Strategy:**
A strict blast radius was established using Docker bridge networks:
* **Public:** Only Port 80 (Nginx) is exposed to the internet.
* **Private (`frontend-network`):** Connects Nginx to the Node.js App.
* **Private (`backend-network`):** Connects the Node.js App to the MySQL database. 

**Learnings & Setbacks:**
Initially, mapping the architecture revealed that exposing port 8080 directly defeated the purpose of a reverse proxy. By strictly placing the app and database on internal Docker networks without host port bindings (`ports:`), we forced all traffic through Nginx, establishing a single, secure entry point.

<img width="1536" height="1024" alt="Architectural Diagram" src="https://github.com/user-attachments/assets/00c0cf41-6890-40da-b70b-397d0bf71aab" />

