# 05 - Configuration and Options

.NET configuration combines values from files, environment variables, command-line arguments, user secrets, and other providers.

Common file:

```json
{
  "Email": {
    "From": "noreply@example.com",
    "RetryCount": 3
  }
}
```

Read directly:

```csharp
string? from = configuration["Email:From"];
```

Prefer options classes for grouped settings:

```csharp
public class EmailOptions
{
    public string From { get; set; } = "";
    public int RetryCount { get; set; }
}
```

```csharp
services.Configure<EmailOptions>(configuration.GetSection("Email"));
```

Configuration should differ by environment without changing code. Keep secrets out of source control.

## Exercises

1. Add an `appsettings.json` file.
2. Read one scalar value.
3. Bind a section to an options class.
4. Override a setting with an environment variable.
5. Store a local secret with user secrets.
