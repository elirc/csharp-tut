# 18 — Dependency Injection & Configuration

Dependency Injection (DI), configuration, and logging are the backbone of every modern .NET application. They're built into the framework (the "generic host"). If you understand this chapter, a lot of ASP.NET Core (Chapter 20) will already make sense. **This is core mid-level knowledge** — DI is assumed on every team.

## Dependency Injection: the idea

A class shouldn't create its own dependencies; it should **receive** them. Instead of `new`-ing collaborators inside, you declare what you need in the constructor and the framework supplies them.

```csharp
// ❌ Tightly coupled — hard to test, hard to change:
public class OrderService
{
    private readonly SqlOrderRepository _repo = new();      // hardwired concrete type
    private readonly SmtpEmailSender _email = new();
}

// ✅ Dependencies injected via constructor, typed as interfaces:
public class OrderService
{
    private readonly IOrderRepository _repo;
    private readonly IEmailSender _email;
    private readonly ILogger<OrderService> _logger;

    public OrderService(IOrderRepository repo, IEmailSender email, ILogger<OrderService> logger)
    {
        _repo = repo;
        _email = email;
        _logger = logger;
    }

    public async Task PlaceAsync(Order order)
    {
        await _repo.AddAsync(order);
        await _email.SendAsync(order.CustomerEmail, "Order confirmed");
        _logger.LogInformation("Placed order {OrderId}", order.Id);
    }
}
```

With C# 12 primary constructors this gets terser:

```csharp
public class OrderService(IOrderRepository repo, IEmailSender email, ILogger<OrderService> logger)
{
    public async Task PlaceAsync(Order order)
    {
        await repo.AddAsync(order);
        await email.SendAsync(order.CustomerEmail, "Order confirmed");
        logger.LogInformation("Placed order {OrderId}", order.Id);
    }
}
```

### Why DI matters

- **Testability** — in tests you inject fakes/mocks (Chapter 21) instead of real databases or email servers.
- **Loose coupling** — depend on `IOrderRepository`, swap the implementation (SQL, in-memory, a different DB) without touching `OrderService`.
- **Lifetime management** — the container creates and disposes objects for you, wiring up the whole graph.

This is "Inversion of Control": you don't control object creation; the container does, based on your registrations.

## The DI container

You register services in a container; it resolves the dependency graph when something is needed. In ASP.NET Core / the generic host, this is set up in `Program.cs`:

```csharp
var builder = WebApplication.CreateBuilder(args);   // (Host.CreateApplicationBuilder for non-web)

// Register services -> what concrete type fulfills each interface:
builder.Services.AddScoped<IOrderRepository, SqlOrderRepository>();
builder.Services.AddScoped<IEmailSender, SmtpEmailSender>();
builder.Services.AddScoped<OrderService>();
builder.Services.AddSingleton<IClock, SystemClock>();

var app = builder.Build();
```

Now when something needs an `OrderService`, the container creates a `SqlOrderRepository`, an `SmtpEmailSender`, a logger, and an `OrderService` wired together — automatically.

## Service lifetimes (know these cold)

How long an instance lives / when a new one is created:

| Lifetime | New instance... | Use for |
|---|---|---|
| **Transient** | every time it's requested | lightweight, stateless services |
| **Scoped** | once per "scope" (per web request) | most app services, **EF Core `DbContext`**, repositories |
| **Singleton** | once for the whole app | stateless shared services, caches, config |

```csharp
builder.Services.AddTransient<IIdGenerator, GuidGenerator>();
builder.Services.AddScoped<IOrderRepository, SqlOrderRepository>();
builder.Services.AddSingleton<IMemoryCache, MemoryCache>();
```

In a web app, **scoped = per HTTP request** — each request gets its own set of scoped services, disposed when the request ends. This is why `DbContext` is scoped: one per request, not shared across requests.

### The captive dependency trap

**Don't inject a shorter-lived service into a longer-lived one.** Injecting a *scoped* `DbContext` into a *singleton* captures it forever, reusing one request's context across all requests — a serious bug. The container warns about some of these; know the rule: a singleton may only depend on singletons; a scoped may depend on scoped/singleton; transient on anything.

## Registration variations

```csharp
// Interface -> implementation:
builder.Services.AddScoped<IEmailSender, SmtpEmailSender>();

// Concrete type (no interface):
builder.Services.AddScoped<OrderService>();

// With a factory (when construction needs logic):
builder.Services.AddSingleton<IClock>(sp => new FixedClock(DateTime.UtcNow));

// Existing instance:
builder.Services.AddSingleton(new MyConfig { Retries = 3 });

// Typed HttpClient (Chapter 17) registers it for you:
builder.Services.AddHttpClient<WeatherApiClient>();

// Many implementations of one interface (inject IEnumerable<IRule>):
builder.Services.AddScoped<IRule, MinLengthRule>();
builder.Services.AddScoped<IRule, NoProfanityRule>();
// then: public Validator(IEnumerable<IRule> rules) { ... }
```

## How dependencies get to you

**Constructor injection** is the standard — declare them as constructor parameters (or primary-constructor params). The framework resolves controllers, services, etc. for you. You almost never manually call the container; let it inject.

If you *must* resolve manually (rare — usually in startup or background services), use the `IServiceProvider`, creating a scope for scoped services:

```csharp
using var scope = app.Services.CreateScope();
var repo = scope.ServiceProvider.GetRequiredService<IOrderRepository>();
```

Avoid the "service locator" anti-pattern (injecting `IServiceProvider` and pulling things out ad hoc) — prefer explicit constructor dependencies.

## Configuration

Configuration is layered from multiple sources, later ones overriding earlier. The default order (highest priority last): `appsettings.json` → `appsettings.{Environment}.json` → environment variables → command-line args → user secrets (in Development).

### `appsettings.json`

```json
{
  "ConnectionStrings": {
    "Default": "Server=localhost;Database=Shop;Trusted_Connection=True;"
  },
  "WeatherApi": {
    "BaseUrl": "https://api.weather.com/",
    "ApiKey": "abc123",
    "TimeoutSeconds": 10
  },
  "Logging": {
    "LogLevel": { "Default": "Information" }
  }
}
```

### Reading config

```csharp
// Direct access:
string? conn = builder.Configuration.GetConnectionString("Default");
string? url = builder.Configuration["WeatherApi:BaseUrl"];      // ':' navigates nesting
int timeout = builder.Configuration.GetValue<int>("WeatherApi:TimeoutSeconds");
```

### The Options pattern (preferred)

Bind a config section to a strongly-typed class and inject it — clean, testable, no magic strings scattered around:

```csharp
public class WeatherApiOptions
{
    public string BaseUrl { get; set; } = "";
    public string ApiKey { get; set; } = "";
    public int TimeoutSeconds { get; set; } = 30;
}

// Register the binding:
builder.Services.Configure<WeatherApiOptions>(
    builder.Configuration.GetSection("WeatherApi"));

// Inject IOptions<T> and read .Value:
public class WeatherApiClient(HttpClient http, IOptions<WeatherApiOptions> options)
{
    private readonly WeatherApiOptions _opts = options.Value;
    // use _opts.BaseUrl, _opts.ApiKey ...
}
```

(`IOptionsSnapshot<T>` re-reads per request; `IOptionsMonitor<T>` reacts to changes — use `IOptions<T>` unless you need reloading.)

### Environments and secrets

- The `ASPNETCORE_ENVIRONMENT` variable (`Development`, `Staging`, `Production`) selects `appsettings.{env}.json`.
- **Never commit secrets** (connection strings with passwords, API keys) to source control. In development use **User Secrets** (`dotnet user-secrets set "WeatherApi:ApiKey" "..."`); in production use environment variables or a secret store (Key Vault, etc.). Config layering makes these override `appsettings.json` automatically.

## Logging

.NET has built-in structured logging via `ILogger<T>`. Inject it and log — don't use `Console.WriteLine` in real services.

```csharp
public class OrderService(ILogger<OrderService> logger)
{
    public void Place(Order order)
    {
        // Structured logging: use message templates with named placeholders,
        // NOT string interpolation — the values become queryable fields.
        logger.LogInformation("Placing order {OrderId} for {CustomerId}",
            order.Id, order.CustomerId);

        try { /* ... */ }
        catch (Exception ex)
        {
            logger.LogError(ex, "Failed to place order {OrderId}", order.Id);
            throw;
        }
    }
}
```

Log levels (least → most severe): `Trace`, `Debug`, `Information`, `Warning`, `Error`, `Critical`. Configure the minimum level per category in `appsettings.json`.

**Important:** use the template form `LogInformation("... {Id}", id)`, *not* `LogInformation($"... {id}")`. The template form preserves structure for log aggregators (Seq, Application Insights, ELK) so you can search/filter by `OrderId`. This is a real mid-level habit.

## Putting it together (a non-web app)

DI/config/logging aren't web-only — the generic host gives them to console/worker apps too:

```csharp
using Microsoft.Extensions.Hosting;
using Microsoft.Extensions.DependencyInjection;

var builder = Host.CreateApplicationBuilder(args);
builder.Services.AddScoped<IOrderRepository, InMemoryOrderRepository>();
builder.Services.AddScoped<OrderService>();

var host = builder.Build();

using var scope = host.Services.CreateScope();
var service = scope.ServiceProvider.GetRequiredService<OrderService>();
await service.PlaceAsync(new Order { /* ... */ });
```

## Exercises

1. Define `IEmailSender` with a console implementation. Register it as scoped and inject it into an `OrderService`. Resolve and run it via the host.
2. Add a `WeatherApiOptions` class bound from `appsettings.json` using the Options pattern, and inject `IOptions<WeatherApiOptions>` into a client.
3. Replace all `Console.WriteLine`s in a service with `ILogger<T>` calls using structured templates, including an error log with the exception in a catch.
4. Demonstrate the captive-dependency problem: try injecting a scoped service into a singleton and observe/explain the issue.

---

**Next:** [19 — Entity Framework Core](./19-entity-framework-core.md)
