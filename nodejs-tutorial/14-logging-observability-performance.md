# 14 - Logging, Observability, and Performance

Production Node apps need useful signals. You cannot debug production by guessing.

Use structured logs:

```js
logger.info({ taskId }, "Task created");
```

Common tools include Pino, Winston, OpenTelemetry, Prometheus, Grafana, cloud logging platforms, and APM tools.

Performance in Node often comes down to avoiding event-loop blocking, streaming large data, measuring latency, controlling concurrency, and understanding memory usage.

Measure before optimizing. A slow API might be blocked by a database query, synchronous file work, excessive JSON parsing, a network dependency, or CPU-heavy code on the event loop.

## Exercises

1. Add structured request logs.
2. Add a health endpoint.
3. Measure request duration.
4. Stream a large file instead of reading all of it.
5. Detect one event-loop blocking operation.
