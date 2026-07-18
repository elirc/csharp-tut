# 14 - Observability and Health

Observability is how you understand a running app without attaching a debugger. The main signals are logs, metrics, traces, and health checks.

Logs answer: what happened?

Metrics answer: how often and how much?

Traces answer: where did this request go?

Health checks answer: should the platform send traffic here?

ASP.NET Core health check example:

```csharp
builder.Services.AddHealthChecks();
app.MapHealthChecks("/health");
```

Modern .NET apps often use OpenTelemetry to collect traces, metrics, and logs for tools such as Application Insights, Grafana, Jaeger, Prometheus, or other platforms.

## Exercises

1. Add a health endpoint.
2. Add structured logs with correlation ids.
3. Add a custom metric.
4. Explore OpenTelemetry basics.
5. Explain the difference between logs, metrics, and traces.
