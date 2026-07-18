# 09 - ASP.NET Core Web APIs

ASP.NET Core is the web framework for .NET. Web APIs expose HTTP endpoints.

Minimal API example:

```csharp
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.MapGet("/api/health", () => Results.Ok(new { ok = true }));

app.Run();
```

Controller example:

```csharp
[ApiController]
[Route("api/[controller]")]
public class TasksController : ControllerBase
{
    [HttpGet]
    public IActionResult GetTasks() => Ok();
}
```

Real APIs need routing, model binding, validation, status codes, error handling, logging, DI, OpenAPI, and tests.

## Exercises

1. Create a Web API.
2. Add a health endpoint.
3. Add GET and POST endpoints.
4. Return correct status codes.
5. Add centralized error handling.
