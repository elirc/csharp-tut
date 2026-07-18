# 06 - Dependency Injection

Dependency injection is the standard way .NET apps provide services to classes.

```csharp
public interface IClock
{
    DateTimeOffset Now { get; }
}

public class SystemClock : IClock
{
    public DateTimeOffset Now => DateTimeOffset.UtcNow;
}
```

Register services:

```csharp
services.AddSingleton<IClock, SystemClock>();
```

Consume services through constructors:

```csharp
public class ReportService(IClock clock)
{
    public DateTimeOffset CreatedAt() => clock.Now;
}
```

Know the lifetimes:

- Transient: new instance every request.
- Scoped: one instance per scope, often one web request.
- Singleton: one instance for the app lifetime.

Most DI bugs come from choosing the wrong lifetime or hiding dependencies.

## Exercises

1. Create an interface and implementation.
2. Register it as transient.
3. Inject it into a service.
4. Change the lifetime and observe behavior.
5. Explain the captive dependency problem.
