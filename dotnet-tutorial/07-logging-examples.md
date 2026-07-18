# 07 - Logging Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `ILogger<T>` | Typed logger. |
| 2 | `LogTrace()` | Very detailed diagnostic logs. |
| 3 | `LogDebug()` | Development/debug logs. |
| 4 | `LogInformation()` | Normal app milestone logs. |
| 5 | `LogWarning()` | Recoverable unusual conditions. |
| 6 | `LogError(ex, "...")` | Failure logs with exception. |
| 7 | `LogCritical()` | Severe failure logs. |
| 8 | `"Order {OrderId}"` | Structured log template. |
| 9 | `BeginScope()` | Adds context to nested logs. |
| 10 | `EventId` | Identifies a log event type. |
| 11 | `appsettings Logging` | Configures levels by category. |
| 12 | `Console provider` | Writes logs to console. |
| 13 | `Debug provider` | Writes logs to debug output. |
| 14 | `category` | Usually logger type name. |
| 15 | `correlation id` | Connects logs from one request. |
| 16 | `do not log secrets` | Avoid leaking sensitive data. |
| 17 | `log exceptions once` | Prevent noisy duplicate error logs. |
| 18 | `use levels intentionally` | Avoid making every log Information. |
| 19 | `structured fields` | Make logs searchable. |
| 20 | `observability starts here` | Logs are one signal among several. |
