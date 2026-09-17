# Pterodactyl Eggs

A collection of Pterodactyl eggs with Docker images built automatically and hosted on GitHub Container Registry (`ghcr.io/kikaku-co-id`).

## Requirements

- Pterodactyl Panel v1.x
- Wings must be able to access the internet to pull images from `ghcr.io/kikaku-co-id`
- (Optional) GitHub Access Token if you want auto-pull from a private repository

## Available Eggs

| Egg | Category | Image | Short Description |
|-----|----------|-------|-------------------|
| [AIO](./Application/egg-a-i-o.json) | Application | `ghcr.io/kikaku-co-id/aio:latest` | All-in-One environment with Node.js, Java, Python, Go, and Git auto-pull. |
| [Komari](./Application/komari.json) | Application | `ghcr.io/kikaku-co-id/komari:latest` | Self-hosted server monitoring panel with web dashboard and agent-based metrics. See [komari.wiki](https://www.komari.wiki/en/). |
| [n8n](./Automation/n8n.json) | Automation | `ghcr.io/kikaku-co-id/n8n-automation:latest` | Workflow automation platform. Runs n8n with Pterodactyl-friendly config. |
| [SQL Server 2022](./Database/mssql.json) | Database | `ghcr.io/kikaku-co-id/sqlserver2022:latest` | Microsoft SQL Server 2022 on Linux, persistent data in the server folder. |

## Startup Setup

How to configure the startup for each egg after importing it into Pterodactyl.

### AIO

Fill the **Startup Command** based on your application needs:

- Interactive mode (console/bash):
  ```
  bash
  ```
- Node.js application:
  ```
  npm i && node index.js
  ```
- Java application:
  ```
  java -jar server.jar
  ```
- Python application:
  ```
  pip install -r requirements.txt && python3 main.py
  ```

**Variables:**

| Variable | Default | Description |
|----------|---------|-------------|
| `Git Repository URL` | *(empty)* | Repository URL to auto-clone/pull. Leave empty to disable. |
| `Automatic Update` | `true` | `true` = automatically git pull and restart when there is an update. |
| `Install Branch` | `main` | Branch to clone and monitor. |
| `Git Access Token` | *(empty)* | Token for private repositories. Never stored on disk. |
| `Check Interval` | `30` | Update check interval in seconds. **Minimum 5 seconds.** |

> Note: if `Startup Command` is set to `bash`, auto-restart on update will not work. Use the application command directly if you want auto-update to stay active.

### Komari

The Komari server listens on the port assigned by Pterodactyl. On first access, follow the installation guide to create the admin account.

| Variable | Default | Description |
|----------|---------|-------------|
| `Gin Mode` | `release` | Gin web framework run mode: `release`, `debug`, or `test`. |

Access the panel via `http://<node-ip>:<assigned-port>` after it starts. Official docs: [https://www.komari.wiki/en/](https://www.komari.wiki/en/)

### n8n

Just fill in the variables below:

| Variable | Default | Description |
|----------|---------|-------------|
| `N8N_ENCRYPTION_KEY` | *(empty)* | Encryption key for sensitive data. Set once and keep it secret. |
| `N8N_SECURE_COOKIE` | `false` | Set to `true` if you use HTTPS. |
| `N8N_PROTOCOL` | `http` | Protocol for webhooks: `http` or `https`. |
| `WEBHOOK_URL` | *(empty)* | Public webhook URL (optional). |

Access n8n via the server's allocated port after it starts.

### SQL Server 2022

Just fill in the variables below:

| Variable | Default | Description |
|----------|---------|-------------|
| `Accept EULA` | `Y` | Must be `Y` to accept the Microsoft license. |
| `SA Password` | *(empty)* | Password for the `sa` account. Minimum 8 characters, must meet SQL Server complexity requirements. |
| `Edition (PID)` | `Developer` | Options: `Developer`, `Express`, `Evaluation`, `Web`, `Standard`, `Enterprise`. |
| `SQL Server Memory Limit (MB)` | *(empty)* | SQL Server memory limit. Leave empty for auto (90% of server allocation). |
| `TCP Port` | `{{SERVER_PORT}}` | Leave default to follow the port assigned by Pterodactyl. |
