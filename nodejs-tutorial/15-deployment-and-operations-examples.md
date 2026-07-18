# 15 - Deployment and Operations Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `"engines": { "node": ">=20" }` | Declares supported Node versions. |
| 2 | `npm ci` | Reproducible dependency install. |
| 3 | `npm ci --omit=dev` | Installs production dependencies only. |
| 4 | `NODE_ENV=production` | Common production environment value. |
| 5 | `PORT` | Common web server port env variable. |
| 6 | `Dockerfile` | Container image recipe. |
| 7 | `.dockerignore` | Excludes files from Docker build context. |
| 8 | `node:lts` | Official Node LTS image tag family. |
| 9 | `process.on("SIGTERM")` | Handles shutdown signal. |
| 10 | `server.close()` | Stops accepting new HTTP connections. |
| 11 | `graceful shutdown` | Finishes work before exiting. |
| 12 | `PM2` | Process manager sometimes used for Node. |
| 13 | `systemd` | Linux service manager. |
| 14 | `Kubernetes` | Container orchestration platform. |
| 15 | `health check` | Signals app availability. |
| 16 | `readiness` | Ready to receive traffic. |
| 17 | `liveness` | Process should stay running. |
| 18 | `CI pipeline` | Builds and tests automatically. |
| 19 | `rollback` | Reverts a bad deploy. |
| 20 | `logs to stdout` | Container-friendly logging convention. |
| 21 | `multi-stage Dockerfile` | Separates build and runtime image stages. |
| 22 | `USER node` | Runs container as non-root user. |
| 23 | `HEALTHCHECK` | Docker instruction for container health. |
| 24 | `SIGINT` | Interrupt signal, often local Ctrl+C. |
| 25 | `SIGTERM` | Common production shutdown signal. |
| 26 | `zero-downtime deploy` | Deploys without interrupting users. |
| 27 | `blue-green deploy` | Switches traffic between two environments. |
| 28 | `rolling deploy` | Gradually replaces running instances. |
| 29 | `horizontal scaling` | Runs more app instances. |
| 30 | `stateless service` | Easier to scale because instance-local data is not required. |

## Interview Questions

**Q1. Why should a Node service handle `SIGTERM`?**  
Platforms send `SIGTERM` during shutdown, and handling it lets the service stop accepting traffic and clean up gracefully.

**Q2. Why should containers log to stdout/stderr?**  
Container platforms collect those streams and route them to centralized logging systems.

**Q3. What is the purpose of `npm ci` in deployment or CI?**  
It performs a clean, reproducible install from the lock file.

**Q4. Why should production services avoid storing important state only in memory?**  
Instances can restart, scale horizontally, or be replaced, causing in-memory state to disappear or diverge.

**Q5. What is the difference between liveness and readiness?**  
Liveness asks whether the process should keep running. Readiness asks whether it can receive traffic right now.
