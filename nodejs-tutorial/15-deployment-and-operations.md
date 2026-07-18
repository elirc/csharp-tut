# 15 - Deployment and Operations

A Node app that works locally still needs production packaging, configuration, process management, logs, health checks, and rollback strategy.

Common production concerns:

- Choose and pin a Node version.
- Use `npm ci` in CI.
- Run tests before deployment.
- Provide configuration through environment variables.
- Do not include dev dependencies in production images unless needed.
- Handle shutdown signals.
- Expose health checks.
- Log to stdout/stderr.
- Monitor memory, CPU, latency, and errors.

Container example:

```dockerfile
FROM node:lts
WORKDIR /app
COPY package*.json ./
RUN npm ci --omit=dev
COPY . .
CMD ["node", "src/server.js"]
```

## Exercises

1. Add an `engines` field.
2. Write a Dockerfile.
3. Handle `SIGTERM`.
4. Add a health endpoint.
5. Run tests in a CI-style command.
