# 16 - Capstone: Production-Shaped Task Service Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `TaskService.Api` | ASP.NET Core entry project. |
| 2 | `TaskService.Core` | Domain and service contracts. |
| 3 | `TaskService.Infrastructure` | EF Core and external integrations. |
| 4 | `TaskService.Tests` | Test project. |
| 5 | `TaskItem entity` | Database/domain record. |
| 6 | `TaskDto` | API response shape. |
| 7 | `CreateTaskRequest` | API input shape. |
| 8 | `TaskService` | Business logic service. |
| 9 | `AppDbContext` | EF Core context. |
| 10 | `AddDbContext` | Registers data access. |
| 11 | `AddScoped<ITaskService, TaskService>()` | Registers business service. |
| 12 | `IOptions<TaskOptions>` | Typed settings. |
| 13 | `ILogger<TaskService>` | Structured service logs. |
| 14 | `MapHealthChecks` | Health endpoint. |
| 15 | `ProblemDetails` | Consistent error responses. |
| 16 | `WebApplicationFactory` | Integration testing host. |
| 17 | `dotnet test` | Runs all tests. |
| 18 | `dotnet publish -c Release` | Publishes service. |
| 19 | `Dockerfile` | Container packaging. |
| 20 | `README runbook` | Explains setup, run, test, and deploy. |
