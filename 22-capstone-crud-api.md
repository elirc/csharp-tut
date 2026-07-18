# 22 — Capstone: A Full CRUD API

Time to assemble everything into one realistic project: a **Task Management API** (a to-do/task tracker with projects). It uses ASP.NET Core, EF Core, DI, DTOs, validation, async, logging, global error handling, and tests. Build this end-to-end — it's the closest thing to a real mid-level deliverable, and finishing it means you've genuinely arrived.

> Chapters referenced: structure (5–7), collections/LINQ (8–10), exceptions (11), async (13), nullability (15), DI/config/logging (18), EF Core (19), Web API (20), testing (21).

## What we're building

A REST API for managing **Projects** and the **TaskItems** within them.

```
GET    /api/projects                 list projects (paged)
GET    /api/projects/{id}            get one project (with task count)
POST   /api/projects                 create a project
PUT    /api/projects/{id}            update a project
DELETE /api/projects/{id}            delete a project

GET    /api/projects/{id}/tasks      list tasks in a project
POST   /api/projects/{id}/tasks      add a task to a project
PUT    /api/tasks/{id}               update a task
PATCH  /api/tasks/{id}/complete      mark a task complete
DELETE /api/tasks/{id}               delete a task
```

## Solution layout

```bash
dotnet new sln -n TaskApi
dotnet new webapi -o src/TaskApi
dotnet new xunit -o tests/TaskApi.Tests
dotnet sln add src/TaskApi tests/TaskApi.Tests
dotnet add tests/TaskApi.Tests reference src/TaskApi

# In src/TaskApi:
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
dotnet add package Microsoft.EntityFrameworkCore.Design
```

We'll keep everything in the one API project for clarity (a larger app would split Core/Data/Api). SQLite keeps it zero-setup.

## 1. Domain entities

```csharp
// Entities/Project.cs
public class Project
{
    public int Id { get; set; }
    public string Name { get; set; } = "";
    public string? Description { get; set; }
    public DateTime CreatedAt { get; set; }
    public List<TaskItem> Tasks { get; set; } = new();
}

// Entities/TaskItem.cs
public enum TaskPriority { Low, Medium, High }

public class TaskItem
{
    public int Id { get; set; }
    public string Title { get; set; } = "";
    public TaskPriority Priority { get; set; }
    public bool IsCompleted { get; set; }
    public DateTime? DueDate { get; set; }

    public int ProjectId { get; set; }
    public Project Project { get; set; } = null!;
}
```

## 2. DbContext

```csharp
// Data/AppDbContext.cs
using Microsoft.EntityFrameworkCore;

public class AppDbContext(DbContextOptions<AppDbContext> options) : DbContext(options)
{
    public DbSet<Project> Projects => Set<Project>();
    public DbSet<TaskItem> Tasks => Set<TaskItem>();

    protected override void OnModelCreating(ModelBuilder b)
    {
        b.Entity<Project>(e =>
        {
            e.Property(p => p.Name).IsRequired().HasMaxLength(200);
            e.Property(p => p.Description).HasMaxLength(1000);
        });
        b.Entity<TaskItem>(e =>
        {
            e.Property(t => t.Title).IsRequired().HasMaxLength(300);
            e.HasOne(t => t.Project)
             .WithMany(p => p.Tasks)
             .HasForeignKey(t => t.ProjectId)
             .OnDelete(DeleteBehavior.Cascade);   // deleting a project deletes its tasks
        });
    }
}
```

## 3. DTOs (records) and validation

```csharp
// Dtos/ProjectDtos.cs
using System.ComponentModel.DataAnnotations;

public record ProjectDto(int Id, string Name, string? Description, int TaskCount, DateTime CreatedAt);

public record CreateProjectRequest(
    [property: Required, StringLength(200)] string Name,
    [property: StringLength(1000)] string? Description);

public record UpdateProjectRequest(
    [property: Required, StringLength(200)] string Name,
    [property: StringLength(1000)] string? Description);

// Dtos/TaskDtos.cs
public record TaskDto(int Id, string Title, TaskPriority Priority, bool IsCompleted, DateTime? DueDate);

public record CreateTaskRequest(
    [property: Required, StringLength(300)] string Title,
    TaskPriority Priority,
    DateTime? DueDate);

public record UpdateTaskRequest(
    [property: Required, StringLength(300)] string Title,
    TaskPriority Priority,
    DateTime? DueDate);
```

Entities never leave the data layer; the API speaks DTOs.

## 4. Domain exceptions + global handler

```csharp
// Exceptions.cs
public class NotFoundException(string message) : Exception(message);
public class ConflictException(string message) : Exception(message);

// Infrastructure/GlobalExceptionHandler.cs
using Microsoft.AspNetCore.Diagnostics;
using Microsoft.AspNetCore.Mvc;

public class GlobalExceptionHandler(ILogger<GlobalExceptionHandler> logger) : IExceptionHandler
{
    public async ValueTask<bool> TryHandleAsync(
        HttpContext ctx, Exception ex, CancellationToken ct)
    {
        var (status, title) = ex switch
        {
            NotFoundException => (StatusCodes.Status404NotFound, ex.Message),
            ConflictException => (StatusCodes.Status409Conflict, ex.Message),
            _ => (StatusCodes.Status500InternalServerError, "An unexpected error occurred.")
        };

        if (status == StatusCodes.Status500InternalServerError)
            logger.LogError(ex, "Unhandled exception");

        ctx.Response.StatusCode = status;
        await ctx.Response.WriteAsJsonAsync(
            new ProblemDetails { Status = status, Title = title }, ct);
        return true;   // handled
    }
}
```

Now services just `throw new NotFoundException(...)` and the boundary turns it into the right HTTP response. Controllers stay clean.

## 5. Services (business logic)

```csharp
// Services/ProjectService.cs
using Microsoft.EntityFrameworkCore;

public class ProjectService(AppDbContext db, ILogger<ProjectService> logger)
{
    public async Task<IReadOnlyList<ProjectDto>> ListAsync(int page, int size, CancellationToken ct)
        => await db.Projects
            .AsNoTracking()
            .OrderBy(p => p.Name)
            .Skip((page - 1) * size).Take(size)
            .Select(p => new ProjectDto(p.Id, p.Name, p.Description, p.Tasks.Count, p.CreatedAt))
            .ToListAsync(ct);

    public async Task<ProjectDto> GetAsync(int id, CancellationToken ct)
    {
        var dto = await db.Projects
            .AsNoTracking()
            .Where(p => p.Id == id)
            .Select(p => new ProjectDto(p.Id, p.Name, p.Description, p.Tasks.Count, p.CreatedAt))
            .FirstOrDefaultAsync(ct);
        return dto ?? throw new NotFoundException($"Project {id} not found");
    }

    public async Task<ProjectDto> CreateAsync(CreateProjectRequest req, CancellationToken ct)
    {
        if (await db.Projects.AnyAsync(p => p.Name == req.Name, ct))
            throw new ConflictException($"A project named '{req.Name}' already exists");

        var project = new Project
        {
            Name = req.Name,
            Description = req.Description,
            CreatedAt = DateTime.UtcNow
        };
        db.Projects.Add(project);
        await db.SaveChangesAsync(ct);
        logger.LogInformation("Created project {ProjectId}", project.Id);
        return new ProjectDto(project.Id, project.Name, project.Description, 0, project.CreatedAt);
    }

    public async Task UpdateAsync(int id, UpdateProjectRequest req, CancellationToken ct)
    {
        var project = await db.Projects.FindAsync([id], ct)
            ?? throw new NotFoundException($"Project {id} not found");
        project.Name = req.Name;
        project.Description = req.Description;
        await db.SaveChangesAsync(ct);
    }

    public async Task DeleteAsync(int id, CancellationToken ct)
    {
        var project = await db.Projects.FindAsync([id], ct)
            ?? throw new NotFoundException($"Project {id} not found");
        db.Projects.Remove(project);
        await db.SaveChangesAsync(ct);
    }
}
```

```csharp
// Services/TaskService.cs
using Microsoft.EntityFrameworkCore;

public class TaskService(AppDbContext db)
{
    public async Task<IReadOnlyList<TaskDto>> ListForProjectAsync(int projectId, CancellationToken ct)
    {
        if (!await db.Projects.AnyAsync(p => p.Id == projectId, ct))
            throw new NotFoundException($"Project {projectId} not found");

        return await db.Tasks
            .AsNoTracking()
            .Where(t => t.ProjectId == projectId)
            .OrderByDescending(t => t.Priority).ThenBy(t => t.DueDate)
            .Select(t => new TaskDto(t.Id, t.Title, t.Priority, t.IsCompleted, t.DueDate))
            .ToListAsync(ct);
    }

    public async Task<TaskDto> CreateAsync(int projectId, CreateTaskRequest req, CancellationToken ct)
    {
        if (!await db.Projects.AnyAsync(p => p.Id == projectId, ct))
            throw new NotFoundException($"Project {projectId} not found");

        var task = new TaskItem
        {
            ProjectId = projectId,
            Title = req.Title,
            Priority = req.Priority,
            DueDate = req.DueDate
        };
        db.Tasks.Add(task);
        await db.SaveChangesAsync(ct);
        return new TaskDto(task.Id, task.Title, task.Priority, task.IsCompleted, task.DueDate);
    }

    public async Task CompleteAsync(int taskId, CancellationToken ct)
    {
        var task = await db.Tasks.FindAsync([taskId], ct)
            ?? throw new NotFoundException($"Task {taskId} not found");
        task.IsCompleted = true;
        await db.SaveChangesAsync(ct);
    }

    public async Task DeleteAsync(int taskId, CancellationToken ct)
    {
        var task = await db.Tasks.FindAsync([taskId], ct)
            ?? throw new NotFoundException($"Task {taskId} not found");
        db.Tasks.Remove(task);
        await db.SaveChangesAsync(ct);
    }
}
```

## 6. Controllers (thin HTTP layer)

```csharp
// Controllers/ProjectsController.cs
using Microsoft.AspNetCore.Mvc;

[ApiController]
[Route("api/projects")]
public class ProjectsController(ProjectService service) : ControllerBase
{
    [HttpGet]
    public async Task<ActionResult<IReadOnlyList<ProjectDto>>> List(
        [FromQuery] int page = 1, [FromQuery] int size = 20, CancellationToken ct = default)
        => Ok(await service.ListAsync(page, size, ct));

    [HttpGet("{id:int}")]
    public async Task<ActionResult<ProjectDto>> Get(int id, CancellationToken ct)
        => Ok(await service.GetAsync(id, ct));   // throws NotFoundException -> 404 via handler

    [HttpPost]
    public async Task<ActionResult<ProjectDto>> Create(CreateProjectRequest req, CancellationToken ct)
    {
        var created = await service.CreateAsync(req, ct);
        return CreatedAtAction(nameof(Get), new { id = created.Id }, created);
    }

    [HttpPut("{id:int}")]
    public async Task<IActionResult> Update(int id, UpdateProjectRequest req, CancellationToken ct)
    {
        await service.UpdateAsync(id, req, ct);
        return NoContent();
    }

    [HttpDelete("{id:int}")]
    public async Task<IActionResult> Delete(int id, CancellationToken ct)
    {
        await service.DeleteAsync(id, ct);
        return NoContent();
    }
}
```

```csharp
// Controllers/TasksController.cs
using Microsoft.AspNetCore.Mvc;

[ApiController]
public class TasksController(TaskService service) : ControllerBase
{
    [HttpGet("api/projects/{projectId:int}/tasks")]
    public async Task<ActionResult<IReadOnlyList<TaskDto>>> List(int projectId, CancellationToken ct)
        => Ok(await service.ListForProjectAsync(projectId, ct));

    [HttpPost("api/projects/{projectId:int}/tasks")]
    public async Task<ActionResult<TaskDto>> Create(int projectId, CreateTaskRequest req, CancellationToken ct)
    {
        var created = await service.CreateAsync(projectId, req, ct);
        return CreatedAtAction(nameof(List), new { projectId }, created);
    }

    [HttpPatch("api/tasks/{id:int}/complete")]
    public async Task<IActionResult> Complete(int id, CancellationToken ct)
    {
        await service.CompleteAsync(id, ct);
        return NoContent();
    }

    [HttpDelete("api/tasks/{id:int}")]
    public async Task<IActionResult> Delete(int id, CancellationToken ct)
    {
        await service.DeleteAsync(id, ct);
        return NoContent();
    }
}
```

Notice how thin controllers are — they bind input, call a service, pick a status code. No business logic, no EF queries, no try/catch.

## 7. Wiring it up: `Program.cs`

```csharp
using Microsoft.EntityFrameworkCore;

var builder = WebApplication.CreateBuilder(args);

builder.Services.AddControllers();
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();

builder.Services.AddDbContext<AppDbContext>(opt =>
    opt.UseSqlite(builder.Configuration.GetConnectionString("Default") ?? "Data Source=tasks.db"));

builder.Services.AddScoped<ProjectService>();
builder.Services.AddScoped<TaskService>();

builder.Services.AddExceptionHandler<GlobalExceptionHandler>();
builder.Services.AddProblemDetails();

var app = builder.Build();

// Apply migrations / create the DB on startup (fine for a sample app):
using (var scope = app.Services.CreateScope())
{
    var db = scope.ServiceProvider.GetRequiredService<AppDbContext>();
    db.Database.EnsureCreated();   // for real apps use db.Database.Migrate();
}

app.UseExceptionHandler();
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}
app.MapControllers();
app.Run();

public partial class Program { }   // expose entry point for integration tests
```

Set up migrations properly with `dotnet ef migrations add Initial` and use `db.Database.Migrate()` instead of `EnsureCreated()` for anything beyond a throwaway demo.

## 8. Run it

```bash
cd src/TaskApi
dotnet run
```

Open Swagger and exercise the endpoints: create a project, add tasks, list them, complete one, delete. Watch the status codes (201, 204, 404, 409).

## 9. Tests

```csharp
// tests/TaskApi.Tests/ProjectServiceTests.cs
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Logging.Abstractions;
using Xunit;

public class ProjectServiceTests
{
    // Each test gets its own isolated SQLite in-memory database.
    private static AppDbContext NewDb()
    {
        var conn = new Microsoft.Data.Sqlite.SqliteConnection("DataSource=:memory:");
        conn.Open();
        var options = new DbContextOptionsBuilder<AppDbContext>().UseSqlite(conn).Options;
        var db = new AppDbContext(options);
        db.Database.EnsureCreated();
        return db;
    }

    [Fact]
    public async Task CreateAsync_NewName_PersistsProject()
    {
        using var db = NewDb();
        var service = new ProjectService(db, NullLogger<ProjectService>.Instance);

        var dto = await service.CreateAsync(new CreateProjectRequest("Website", null), default);

        Assert.True(dto.Id > 0);
        Assert.Equal("Website", dto.Name);
        Assert.Equal(1, await db.Projects.CountAsync());
    }

    [Fact]
    public async Task CreateAsync_DuplicateName_ThrowsConflict()
    {
        using var db = NewDb();
        var service = new ProjectService(db, NullLogger<ProjectService>.Instance);
        await service.CreateAsync(new CreateProjectRequest("Website", null), default);

        await Assert.ThrowsAsync<ConflictException>(
            () => service.CreateAsync(new CreateProjectRequest("Website", null), default));
    }

    [Fact]
    public async Task GetAsync_MissingId_ThrowsNotFound()
    {
        using var db = NewDb();
        var service = new ProjectService(db, NullLogger<ProjectService>.Instance);

        await Assert.ThrowsAsync<NotFoundException>(() => service.GetAsync(999, default));
    }
}
```

```csharp
// tests/TaskApi.Tests/ProjectsApiTests.cs — integration test through the full stack
using System.Net;
using System.Net.Http.Json;
using Microsoft.AspNetCore.Mvc.Testing;
using Xunit;

public class ProjectsApiTests(WebApplicationFactory<Program> factory)
    : IClassFixture<WebApplicationFactory<Program>>
{
    private readonly HttpClient _client = factory.CreateClient();

    [Fact]
    public async Task Create_then_Get_RoundTrips()
    {
        var post = await _client.PostAsJsonAsync("/api/projects",
            new CreateProjectRequest("Q3 Launch", "marketing"));
        Assert.Equal(HttpStatusCode.Created, post.StatusCode);

        var created = await post.Content.ReadFromJsonAsync<ProjectDto>();
        var fetched = await _client.GetFromJsonAsync<ProjectDto>($"/api/projects/{created!.Id}");
        Assert.Equal("Q3 Launch", fetched!.Name);
    }

    [Fact]
    public async Task Get_Missing_Returns404()
    {
        var resp = await _client.GetAsync("/api/projects/999999");
        Assert.Equal(HttpStatusCode.NotFound, resp.StatusCode);
    }
}
```

```bash
dotnet test
```

## What you just demonstrated

Walking through this, you used essentially the whole tutorial:

- **Records & DTOs** to model the API contract (Ch. 7), **enums** for priority.
- **Classes, properties, primary constructors** for entities and services (Ch. 5, 14).
- **LINQ** translated to SQL for filtering/sorting/projection/paging (Ch. 10, 19).
- **async/await + CancellationToken** through every layer (Ch. 13).
- **Nullable reference types** guiding null handling (Ch. 15).
- **DI, configuration, structured logging** wiring the app together (Ch. 18).
- **EF Core** with migrations, relationships, change tracking, `AsNoTracking` (Ch. 19).
- **ASP.NET Core** controllers, routing, model validation, REST status codes, global error handling (Ch. 20).
- **xUnit** unit tests (isolated DB) and integration tests (full stack) (Ch. 21).

That layered, tested, async, DI-driven CRUD API *is* the shape of typical mid-level backend C# work in most companies.

## Where to go next

You're now mid-level. To keep growing:
- **Authentication/Authorization** — JWT, ASP.NET Identity, OAuth/OIDC.
- **Caching** — `IMemoryCache`, distributed cache (Redis).
- **Background work** — `IHostedService`/`BackgroundService`, message queues.
- **Resilience** — Polly, retries, circuit breakers, idempotency.
- **Performance** — profiling, `Span<T>`, `IAsyncEnumerable`, EF query tuning.
- **Architecture** — clean/onion/vertical-slice architecture, CQRS, MediatR, domain-driven design.
- **Observability** — OpenTelemetry, structured logging sinks, metrics, tracing.
- **Deployment** — Docker, CI/CD, cloud (Azure/AWS), configuration & secrets in production.

Pick these up as your work demands them — but the core competence is in your hands now. Build things, read other people's code, and keep the compiler warnings at zero.

---

**Back to the [README index](./README.md).**
