# 20 — ASP.NET Core Web API

This is where it all comes together: building the HTTP API that fronts your data. **ASP.NET Core** is the framework for web apps and APIs in .NET. We'll build a REST API using everything from earlier chapters — DI, async, EF Core, DTOs. This chapter covers controllers (the mainstream style) and touches minimal APIs.

## Creating the project

```bash
dotnet new webapi -o Shop.Api
cd Shop.Api
dotnet run
```

The template gives you a working API with Swagger/OpenAPI. Open the URL it prints to see the interactive docs.

## `Program.cs` — the composition root

Modern ASP.NET Core configures everything in one file. This is where DI registration (Chapter 18) and the request pipeline live:

```csharp
var builder = WebApplication.CreateBuilder(args);

// --- Register services (DI container) ---
builder.Services.AddControllers();
builder.Services.AddDbContext<ShopDbContext>(opt =>
    opt.UseSqlServer(builder.Configuration.GetConnectionString("Default")));
builder.Services.AddScoped<CustomerService>();
builder.Services.AddScoped<ICustomerRepository, CustomerRepository>();
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();          // OpenAPI/Swagger docs

var app = builder.Build();

// --- Configure the HTTP pipeline (middleware, order matters) ---
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}
app.UseHttpsRedirection();
app.UseAuthentication();
app.UseAuthorization();
app.MapControllers();                      // wire up controller routes

app.Run();
```

Two halves: **register services** (before `Build()`), then **configure the pipeline** (after). The pipeline is a chain of **middleware** — each component can act on the request and pass it along.

## Middleware: the request pipeline

Every request flows through ordered middleware: HTTPS redirect → routing → auth → your endpoint → back out. Order matters (auth must come before the endpoint). You'll mostly use built-in middleware; occasionally write your own (e.g., for global error handling, request logging). Built-in pieces you'll see: `UseHttpsRedirection`, `UseAuthentication`, `UseAuthorization`, `UseCors`, `UseExceptionHandler`.

## Controllers and actions

A **controller** groups related endpoints. Actions are methods mapped to HTTP verbs + routes. This is the conventional, scalable style for non-trivial APIs.

```csharp
using Microsoft.AspNetCore.Mvc;

[ApiController]                          // enables automatic model validation, binding niceties
[Route("api/[controller]")]              // base route -> /api/customers
public class CustomersController : ControllerBase
{
    private readonly CustomerService _service;
    public CustomersController(CustomerService service) => _service = service;

    // GET /api/customers?page=1&size=20
    [HttpGet]
    public async Task<ActionResult<IReadOnlyList<CustomerDto>>> GetAll(
        [FromQuery] int page = 1, [FromQuery] int size = 20, CancellationToken ct = default)
    {
        var customers = await _service.ListAsync(page, size, ct);
        return Ok(customers);
    }

    // GET /api/customers/42
    [HttpGet("{id:int}")]
    public async Task<ActionResult<CustomerDto>> GetById(int id, CancellationToken ct)
    {
        var customer = await _service.GetAsync(id, ct);
        return customer is null ? NotFound() : Ok(customer);
    }

    // POST /api/customers
    [HttpPost]
    public async Task<ActionResult<CustomerDto>> Create(
        [FromBody] CreateCustomerRequest request, CancellationToken ct)
    {
        var created = await _service.CreateAsync(request, ct);
        // 201 Created with a Location header pointing at the new resource:
        return CreatedAtAction(nameof(GetById), new { id = created.Id }, created);
    }

    // PUT /api/customers/42
    [HttpPut("{id:int}")]
    public async Task<IActionResult> Update(int id, [FromBody] UpdateCustomerRequest request, CancellationToken ct)
    {
        var updated = await _service.UpdateAsync(id, request, ct);
        return updated ? NoContent() : NotFound();
    }

    // DELETE /api/customers/42
    [HttpDelete("{id:int}")]
    public async Task<IActionResult> Delete(int id, CancellationToken ct)
    {
        var deleted = await _service.DeleteAsync(id, ct);
        return deleted ? NoContent() : NotFound();
    }
}
```

Key points:
- `[ApiController]` + `ControllerBase` = API controller (no views).
- `[Route("api/[controller]")]` — `[controller]` is replaced by the class name minus "Controller" (`customers`).
- `[HttpGet]`, `[HttpPost]`, etc. map verbs; route templates like `"{id:int}"` capture and constrain parameters.
- Controllers and their dependencies are resolved by **DI** per request.

## Model binding and sources

ASP.NET Core maps the HTTP request to your action parameters automatically:
- `[FromRoute]` — from the URL path (`{id}`). Usually inferred.
- `[FromQuery]` — from the query string (`?page=1`).
- `[FromBody]` — from the JSON request body (deserialized via System.Text.Json). One per action.
- `[FromHeader]`, `[FromForm]`, `[FromServices]` — headers, form data, DI.

With `[ApiController]`, binding sources are mostly inferred (complex types → body, simple types → route/query), so you write less.

## DTOs and validation

**Never expose EF entities directly.** Define request/response DTOs (records are perfect) — they decouple your API contract from your DB schema, prevent over-posting, and avoid serialization cycles.

```csharp
public record CreateCustomerRequest(
    [property: Required, StringLength(200)] string Name,
    [property: Required, EmailAddress] string Email);

public record CustomerDto(int Id, string Name, string Email);
```

With `[ApiController]`, **validation runs automatically**: if the incoming model violates data-annotation attributes (`[Required]`, `[EmailAddress]`, `[Range]`, `[StringLength]`), the framework returns a `400 Bad Request` with a structured error body *before* your action runs. You don't write `if (!ModelState.IsValid)` — it's handled. For complex rules, the **FluentValidation** library is the popular choice.

## Returning the right responses

Use the helper methods on `ControllerBase` to return correct HTTP semantics:

| Method | Status | When |
|---|---|---|
| `Ok(value)` | 200 | Successful GET/PUT with a body |
| `CreatedAtAction(...)` | 201 | Successful POST that created a resource |
| `NoContent()` | 204 | Successful PUT/DELETE with no body |
| `BadRequest(...)` | 400 | Invalid input |
| `NotFound()` | 404 | Resource doesn't exist |
| `Conflict(...)` | 409 | Duplicate / state conflict |
| `Unauthorized()` / `Forbid()` | 401 / 403 | Auth failures |

Returning proper status codes is REST literacy — a mid-level dev gets these right. `ActionResult<T>` lets you return either a typed value or a status result.

## Global error handling

Don't wrap every action in try/catch. Handle exceptions centrally so unhandled errors become clean responses (and don't leak stack traces). The modern way uses `IExceptionHandler` + `app.UseExceptionHandler`, or the `ProblemDetails` middleware:

```csharp
builder.Services.AddProblemDetails();
// ...
app.UseExceptionHandler();   // unhandled exceptions -> RFC 7807 ProblemDetails JSON
```

You map your domain exceptions (e.g., `NotFoundException` → 404, `ConflictException` → 409) in an `IExceptionHandler`. Business code just throws; the boundary translates. This keeps controllers thin.

## Layered architecture (how real apps are organized)

A typical API separates concerns:

```
Controller   -> HTTP concerns: routing, status codes, model binding. Thin.
   ↓ calls
Service      -> business logic, orchestration, validation rules.
   ↓ uses
Repository / DbContext -> data access (EF Core).
   ↓
Database
```

- **Controllers** are thin — translate HTTP ↔ service calls, pick status codes.
- **Services** hold business logic, are unit-testable (inject fakes).
- **Data layer** (DbContext or repositories) talks to the DB.
- **DTOs** cross the boundaries; **entities** stay in the data layer.

This separation is exactly what makes the code testable and maintainable, and it's what teams expect. Don't put EF queries or business rules in controllers.

## Minimal APIs (the lightweight alternative)

For small services, ASP.NET Core supports defining endpoints inline in `Program.cs` without controllers:

```csharp
var app = builder.Build();

app.MapGet("/api/customers/{id:int}", async (int id, CustomerService service, CancellationToken ct) =>
{
    var customer = await service.GetAsync(id, ct);
    return customer is null ? Results.NotFound() : Results.Ok(customer);
});

app.MapPost("/api/customers", async (CreateCustomerRequest req, CustomerService service, CancellationToken ct) =>
{
    var created = await service.CreateAsync(req, ct);
    return Results.Created($"/api/customers/{created.Id}", created);
});

app.Run();
```

Note DI still works — parameters like `CustomerService` are injected per request. Minimal APIs are great for microservices and small surfaces; controllers scale better for large APIs with lots of endpoints and shared conventions. Know both; teams use either.

## Other things you'll meet

- **CORS** — `builder.Services.AddCors(...)` + `app.UseCors(...)` to allow browser clients from other origins.
- **Authentication/Authorization** — JWT bearer tokens are the norm for APIs: `AddAuthentication().AddJwtBearer(...)`, then `[Authorize]` on controllers/actions. `[AllowAnonymous]` opts out.
- **Versioning** — `Asp.Versioning` package for `/api/v1/...`.
- **Swagger/OpenAPI** — auto-generated interactive docs; invaluable for testing and client generation.
- **Health checks** — `AddHealthChecks()` + `MapHealthChecks("/health")` for load balancers/orchestrators.

## Exercises

1. Scaffold a `webapi` project and add a `CustomersController` with full CRUD backed by the `CustomerService` from Chapter 19 (use SQLite).
2. Add `CreateCustomerRequest` with validation attributes and confirm a bad request returns 400 automatically (test via Swagger).
3. Make `POST` return `201 CreatedAtAction` with a Location header, and `DELETE` return `204`/`404` appropriately.
4. Add a `NotFoundException`/`ConflictException` and global exception handling that maps them to 404/409. Throw them from the service and verify the responses.
5. Re-implement one endpoint as a minimal API and compare.

---

**Next:** [21 — Testing](./21-testing.md)
