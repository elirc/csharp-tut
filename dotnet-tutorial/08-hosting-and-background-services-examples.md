# 08 - Hosting and Background Services Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `Host.CreateApplicationBuilder(args)` | Creates a generic host builder. |
| 2 | `builder.Services` | Registers services. |
| 3 | `builder.Configuration` | Reads configuration. |
| 4 | `builder.Logging` | Configures logging. |
| 5 | `builder.Build()` | Builds the host. |
| 6 | `host.RunAsync()` | Runs until shutdown. |
| 7 | `IHostedService` | Start/stop service contract. |
| 8 | `BackgroundService` | Base class for long-running workers. |
| 9 | `ExecuteAsync()` | Main background loop method. |
| 10 | `CancellationToken` | Shutdown signal. |
| 11 | `stoppingToken.IsCancellationRequested` | Checks for shutdown. |
| 12 | `Task.Delay(..., token)` | Delay that cooperates with cancellation. |
| 13 | `AddHostedService<Worker>()` | Registers a hosted service. |
| 14 | `IHostApplicationLifetime` | App lifetime notifications. |
| 15 | `StartAsync()` | Hosted service startup method. |
| 16 | `StopAsync()` | Hosted service shutdown method. |
| 17 | `scoped dependency in worker` | Requires creating a scope. |
| 18 | `timer loop` | Common worker pattern. |
| 19 | `queue consumer` | Common worker pattern. |
| 20 | `graceful shutdown` | Stop work cleanly on cancellation. |
