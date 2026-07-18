# 06 - Dependency Injection Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `IServiceCollection` | Registration collection. |
| 2 | `IServiceProvider` | Runtime service resolver. |
| 3 | `AddTransient<IService, Service>()` | New instance each resolution. |
| 4 | `AddScoped<IService, Service>()` | One instance per scope. |
| 5 | `AddSingleton<IService, Service>()` | One instance for app lifetime. |
| 6 | `GetRequiredService<T>()` | Resolves or throws. |
| 7 | `CreateScope()` | Creates a service scope manually. |
| 8 | `constructor injection` | Dependencies arrive through constructors. |
| 9 | `factory registration` | Uses a lambda to create a service. |
| 10 | `TryAddSingleton` | Adds only if not already registered. |
| 11 | `IEnumerable<TService>` | Resolves all registrations for a service type. |
| 12 | `keyed services` | Named service registrations in newer .NET. |
| 13 | `captive dependency` | Singleton holds a scoped service. |
| 14 | `composition root` | Startup area where services are wired. |
| 15 | `ActivatorUtilities` | Creates objects while filling DI dependencies. |
| 16 | `ValidateScopes` | Catches some lifetime mistakes. |
| 17 | `ValidateOnBuild` | Validates service graph during startup. |
| 18 | `avoid service locator` | Prefer constructor dependencies over resolving everywhere. |
| 19 | `small interfaces` | Easier to test and replace. |
| 20 | `DI container` | Infrastructure that creates object graphs. |
