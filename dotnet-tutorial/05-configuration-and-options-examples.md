# 05 - Configuration and Options Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `appsettings.json` | Common configuration file. |
| 2 | `appsettings.Development.json` | Environment-specific configuration file. |
| 3 | `configuration["Email:From"]` | Reads nested config value. |
| 4 | `GetSection("Email")` | Selects a config section. |
| 5 | `GetValue<int>("RetryCount")` | Reads and converts a scalar value. |
| 6 | `GetConnectionString("Default")` | Reads a named connection string. |
| 7 | `services.Configure<TOptions>(section)` | Binds options to configuration. |
| 8 | `IOptions<T>` | Reads configured options. |
| 9 | `IOptionsSnapshot<T>` | Scoped options that can refresh between requests. |
| 10 | `IOptionsMonitor<T>` | Observes options changes. |
| 11 | `ValidateDataAnnotations()` | Validates options with attributes. |
| 12 | `ValidateOnStart()` | Fails startup when options are invalid. |
| 13 | `dotnet user-secrets init` | Enables local user secrets. |
| 14 | `dotnet user-secrets set "ApiKey" "value"` | Stores a local secret. |
| 15 | `Environment.GetEnvironmentVariable()` | Reads raw environment variable. |
| 16 | `ASPNETCORE_ENVIRONMENT` | Common ASP.NET Core environment variable. |
| 17 | `DOTNET_ENVIRONMENT` | Generic host environment variable. |
| 18 | `CommandLine provider` | Allows command-line config overrides. |
| 19 | `Azure Key Vault` | Production secret provider option. |
| 20 | `Options pattern` | Strongly typed configuration approach. |
