# 14 - Logging, Observability, and Performance Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `console.log()` | Basic logging. |
| 2 | `console.error()` | Error logging. |
| 3 | `pino` | Fast structured logger. |
| 4 | `winston` | Flexible logging library. |
| 5 | `request id` | Correlates logs for one request. |
| 6 | `trace id` | Correlates distributed work. |
| 7 | `OpenTelemetry` | Standard observability tooling. |
| 8 | `/health` | Health check endpoint. |
| 9 | `process.memoryUsage()` | Reads memory stats. |
| 10 | `process.cpuUsage()` | Reads CPU usage. |
| 11 | `performance.now()` | Measures elapsed time. |
| 12 | `console.time()` | Simple timing helper. |
| 13 | `clinic.js` | Node performance analysis tool family. |
| 14 | `flamegraph` | Visual CPU profile. |
| 15 | `event loop lag` | Delay caused by blocking work. |
| 16 | `stream large data` | Avoids loading all data into memory. |
| 17 | `connection pool` | Controls DB connection reuse. |
| 18 | `concurrency limit` | Prevents overwhelming dependencies. |
| 19 | `metrics` | Numeric operational measurements. |
| 20 | `alerting` | Notifies when signals cross thresholds. |
| 21 | `pino-http` | HTTP request logging middleware for Pino. |
| 22 | `structured JSON logs` | Machine-readable logs for production systems. |
| 23 | `log level` | Controls verbosity such as info, warn, or error. |
| 24 | `redaction` | Removes sensitive fields from logs. |
| 25 | `AsyncLocalStorage` | Stores request context across async calls. |
| 26 | `diagnostics_channel` | Node API for diagnostic instrumentation. |
| 27 | `perf_hooks` | Built-in performance measurement APIs. |
| 28 | `heap snapshot` | Captures memory state for leak analysis. |
| 29 | `load test` | Measures behavior under traffic. |
| 30 | `SLO` | Service-level objective for reliability targets. |

## Interview Questions

**Q1. What is structured logging?**  
It is logging data as fields, often JSON, so logs can be searched, filtered, and correlated.

**Q2. What is event-loop lag?**  
It is delay in the event loop caused by blocking work, which can make timers and requests respond late.

**Q3. Why measure before optimizing?**  
Performance bottlenecks are often not where developers guess. Measurement identifies the real problem.

**Q4. What is the difference between logs, metrics, and traces?**  
Logs describe events, metrics measure values over time, and traces follow work across services or components.

**Q5. Why can reading a huge file with `readFile` hurt a service?**  
It can consume large memory and delay other work; streaming is safer for large data.
