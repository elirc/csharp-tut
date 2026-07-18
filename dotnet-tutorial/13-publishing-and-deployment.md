# 13 - Publishing and Deployment

Building compiles code. Publishing prepares output to run somewhere else.

```bash
dotnet publish -c Release -o ./publish
```

Deployment targets vary: a folder, Windows service, Linux service, Docker container, Azure App Service, Kubernetes, serverless, or another hosting platform.

Publishing choices matter:

- Framework-dependent: target machine has .NET installed.
- Self-contained: publish includes the runtime.
- Single-file: app can be bundled into one executable.
- Trimmed: unused code is removed, but compatibility must be tested.
- ReadyToRun/AOT: startup/performance tradeoffs.

Production apps also need environment-specific configuration, secrets, logging, health checks, database migrations, and rollback plans.

## Exercises

1. Publish a release build.
2. Run the published app.
3. Publish framework-dependent output.
4. Publish self-contained output.
5. Write a simple Dockerfile.
