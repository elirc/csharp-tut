# 16 - Capstone: Production-Shaped Task Service

Build a small task service that uses the .NET platform, not just C# syntax.

Suggested solution:

```text
TaskService.sln
src/TaskService.Api
src/TaskService.Core
src/TaskService.Infrastructure
tests/TaskService.Tests
```

Required features:

- ASP.NET Core Web API.
- EF Core persistence.
- DTOs and validation.
- Dependency injection.
- Configuration with options.
- Structured logging.
- Health checks.
- Typed HTTP client for one external dependency or fake integration.
- Unit tests and integration tests.
- Publish profile or Dockerfile.

The goal is production shape, not production complexity. Keep the domain small, but include the platform concerns that real .NET work requires.

## Exercises

1. Create the solution and projects.
2. Add task CRUD endpoints.
3. Add EF Core migrations.
4. Add health checks and logging.
5. Add unit and integration tests.
6. Publish or containerize the service.
