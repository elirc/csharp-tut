# 08 - Hosting and Background Services

The .NET Generic Host wires configuration, logging, dependency injection, lifetime events, and hosted services.

```csharp
HostApplicationBuilder builder = Host.CreateApplicationBuilder(args);

builder.Services.AddHostedService<Worker>();

using IHost host = builder.Build();
await host.RunAsync();
```

Background services are useful for queue processing, scheduled work, polling, and long-running tasks.

```csharp
public class Worker(ILogger<Worker> logger) : BackgroundService
{
    protected override async Task ExecuteAsync(CancellationToken stoppingToken)
    {
        while (!stoppingToken.IsCancellationRequested)
        {
            logger.LogInformation("Working");
            await Task.Delay(TimeSpan.FromSeconds(5), stoppingToken);
        }
    }
}
```

Hosted services should respect cancellation and avoid blocking shutdown.

## Exercises

1. Create a worker service.
2. Add a `BackgroundService`.
3. Inject a dependency.
4. Use `CancellationToken`.
5. Add structured logs.
