# 07 - Logging

.NET logging is built around `ILogger<T>`.

```csharp
public class Worker(ILogger<Worker> logger)
{
    public void Run()
    {
        logger.LogInformation("Worker started at {Time}", DateTimeOffset.UtcNow);
    }
}
```

Use structured logging templates instead of string interpolation:

```csharp
logger.LogInformation("Created order {OrderId}", orderId);
```

This lets log providers store `OrderId` as a queryable field.

Common levels:

- Trace
- Debug
- Information
- Warning
- Error
- Critical

Good logs explain what happened, include useful context, and avoid secrets or sensitive data.

## Exercises

1. Inject `ILogger<T>`.
2. Log at information, warning, and error levels.
3. Add structured properties.
4. Add a logging scope.
5. Remove sensitive data from a log message.
