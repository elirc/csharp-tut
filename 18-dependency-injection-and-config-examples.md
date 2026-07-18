# 18 - Dependency Injection & Configuration Examples

These snippets reinforce constructor injection, service registration, lifetimes, options, environments, secrets, logging, and non-web hosting.

## Example 1 - Constructor injection

```csharp
public class OrderService
{
    private readonly INotifier _notifier;
    public OrderService(INotifier notifier) => _notifier = notifier;
}
```

Constructor injection makes dependencies explicit and keeps classes easier to test.

## Example 2 - Interface dependency

```csharp
public interface INotifier
{
    void Send(string message);
}
```

Interfaces describe what the class needs without tying it to one implementation.

## Example 3 - Concrete implementation

```csharp
public class EmailNotifier : INotifier
{
    public void Send(string message) => Console.WriteLine($"Email: {message}");
}
```

The implementation contains the actual behavior behind the interface.

## Example 4 - Register transient

```csharp
services.AddTransient<IProcessor, Processor>();
```

Transient services are created every time they are requested.

## Example 5 - Register scoped

```csharp
services.AddScoped<IUnitOfWork, UnitOfWork>();
```

Scoped services are shared within one scope, such as one web request.

## Example 6 - Register singleton

```csharp
services.AddSingleton<ISystemClock, SystemClock>();
```

Singleton services are created once and shared for the app lifetime.

## Example 7 - Captive dependency warning

```csharp
services.AddSingleton<ReportCache>();
services.AddScoped<RequestContext>();
```

A singleton should not hold a scoped service because the scoped instance may outlive its intended scope.

## Example 8 - Factory registration

```csharp
services.AddSingleton<IDGenerator>(_ => new IdGenerator(Guid.NewGuid().ToString()));
```

Factory registrations are useful when construction needs custom logic.

## Example 9 - Register existing instance

```csharp
services.AddSingleton(new AppVersion("1.0.0"));
```

An existing object can be registered directly as a singleton instance.

## Example 10 - Resolve from provider

```csharp
using ServiceProvider provider = services.BuildServiceProvider();
var service = provider.GetRequiredService<OrderService>();
```

`GetRequiredService` retrieves a service or throws when it is missing.

## Example 11 - appsettings JSON

```json
{
  "Greeting": "Hello",
  "Email": {
    "From": "noreply@example.com"
  }
}
```

Configuration is commonly stored as hierarchical JSON.

## Example 12 - Read configuration value

```csharp
string greeting = configuration["Greeting"] ?? "Hi";
```

Indexing into configuration reads a string value by key.

## Example 13 - Read nested configuration

```csharp
string from = configuration["Email:From"] ?? "missing@example.com";
```

Colons navigate nested configuration sections.

## Example 14 - Options class

```csharp
public class EmailOptions
{
    public string From { get; set; } = "";
}
```

Options classes give configuration strongly typed names and defaults.

## Example 15 - Bind options

```csharp
services.Configure<EmailOptions>(configuration.GetSection("Email"));
```

The options pattern maps a configuration section onto a class.

## Example 16 - Consume options

```csharp
public class Mailer(IOptions<EmailOptions> options)
{
    private readonly EmailOptions _options = options.Value;
}
```

`IOptions<T>` gives services access to strongly typed configuration.

## Example 17 - Environment name

```csharp
if (environment.IsDevelopment())
{
    Console.WriteLine("Development mode");
}
```

Environment checks let apps change behavior between development and production.

## Example 18 - User secrets command

```powershell
dotnet user-secrets set "Email:ApiKey" "local-secret"
```

User secrets keep local development secrets out of source-controlled JSON files.

## Example 19 - Logging injection

```csharp
public class Worker(ILogger<Worker> logger)
{
    public void Run() => logger.LogInformation("Worker started");
}
```

Typed loggers attach the class name as logging context.

## Example 20 - Generic host

```csharp
using IHost host = Host.CreateDefaultBuilder(args)
    .ConfigureServices(services =>
    {
        services.AddTransient<OrderService>();
    })
    .Build();
```

The generic host wires configuration, logging, and DI for non-web apps.

Helper types used above:

```csharp
public interface IProcessor { }
public class Processor : IProcessor { }
public interface IUnitOfWork { }
public class UnitOfWork : IUnitOfWork { }
public interface ISystemClock { }
public class SystemClock : ISystemClock { }
public class ReportCache { }
public class RequestContext { }
public interface IDGenerator { }
public class IdGenerator(string prefix) : IDGenerator;
public record AppVersion(string Value);
```

## Additional Reinforcement Examples

| # | Snippet | Explanation |
|---|---|---|
| 21 | `services.AddTransient<IEmailSender, EmailSender>();` | Creates a new service instance for every request. |
| 22 | `services.AddScoped<IOrderContext, OrderContext>();` | Shares one instance within a scope. |
| 23 | `services.AddSingleton<ICache, MemoryCache>();` | Shares one instance for the app lifetime. |
| 24 | `services.TryAddSingleton<IClock, SystemClock>();` | Adds a service only if one is not already registered. |
| 25 | `services.AddHostedService<Worker>();` | Registers a background service in a hosted app. |
| 26 | `services.AddOptions<EmailOptions>();` | Registers options support for a specific options type. |
| 27 | `services.Configure<EmailOptions>(section);` | Binds configuration to an options class. |
| 28 | `services.PostConfigure<EmailOptions>(o => { })` | Adjusts options after normal configuration. |
| 29 | `services.AddOptions<EmailOptions>().Bind(section).ValidateDataAnnotations();` | Binds and validates options. |
| 30 | `IOptions<EmailOptions>` | Reads options as a singleton-friendly snapshot. |
| 31 | `IOptionsSnapshot<EmailOptions>` | Reads scoped options that can update between requests. |
| 32 | `IOptionsMonitor<EmailOptions>` | Observes option changes over time. |
| 33 | `configuration.GetSection("Email")` | Selects a nested configuration section. |
| 34 | `configuration.GetValue<int>("RetryCount")` | Reads and converts a scalar configuration value. |
| 35 | `configuration.GetConnectionString("Default")` | Reads a connection string by name. |
| 36 | `builder.Configuration.AddJsonFile("appsettings.Local.json", optional: true)` | Adds an optional local configuration file. |
| 37 | `builder.Configuration.AddEnvironmentVariables()` | Allows environment variables to override configuration. |
| 38 | `builder.Configuration.AddUserSecrets<Program>()` | Adds development secrets outside source control. |
| 39 | `builder.Environment.IsDevelopment()` | Checks the current app environment. |
| 40 | `builder.Environment.IsProduction()` | Checks whether the app is running in production. |
| 41 | `logger.LogTrace("Details")` | Logs very detailed diagnostic information. |
| 42 | `logger.LogDebug("Debugging")` | Logs development-focused details. |
| 43 | `logger.LogInformation("Started")` | Logs normal application milestones. |
| 44 | `logger.LogWarning("Retrying")` | Logs unusual but recoverable situations. |
| 45 | `logger.LogError(ex, "Failed")` | Logs failures with exception details. |
| 46 | `logger.LogCritical(ex, "Stopping")` | Logs severe failures that may stop the app. |
| 47 | `using var scope = logger.BeginScope("Order {OrderId}", id);` | Adds structured context to all logs in a scope. |
| 48 | `provider.CreateScope()` | Creates a DI scope manually in non-web code. |
| 49 | `scope.ServiceProvider.GetRequiredService<T>()` | Resolves scoped services inside a manual scope. |
| 50 | `ActivatorUtilities.CreateInstance<T>(provider)` | Creates an object while letting DI fill constructor dependencies. |
| 51 | `IServiceProvider` | The runtime service resolver behind DI. |
| 52 | `IServiceCollection` | The registration list used while configuring DI. |
| 53 | `IHostedService` | Contract for services started and stopped by the host. |
| 54 | `BackgroundService` | Base class for long-running hosted workers. |
| 55 | `Host.CreateApplicationBuilder(args)` | Modern starting point for generic host apps. |
| 56 | `builder.Services.AddLogging()` | Registers logging services. |
| 57 | `builder.Services.AddHttpClient()` | Registers HTTP client factory services. |
| 58 | `builder.Build()` | Builds the host or app after registrations are complete. |
| 59 | `await host.RunAsync();` | Starts a host and waits for shutdown. |
| 60 | `await host.StopAsync();` | Requests graceful host shutdown. |
