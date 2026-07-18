# 03 - Process, Environment, and CLI Args Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `process.cwd()` | Current working directory. |
| 2 | `process.argv` | Full command-line argument array. |
| 3 | `process.argv.slice(2)` | User-provided CLI args. |
| 4 | `process.env.PORT` | Reads environment variable. |
| 5 | `Number(process.env.PORT ?? 3000)` | Parses numeric config with fallback. |
| 6 | `process.exitCode = 1` | Marks process as failed. |
| 7 | `process.pid` | Current process id. |
| 8 | `process.platform` | Current operating system platform. |
| 9 | `process.version` | Node version string. |
| 10 | `process.versions` | Versions of Node internals. |
| 11 | `process.memoryUsage()` | Runtime memory usage. |
| 12 | `process.uptime()` | Seconds since process started. |
| 13 | `process.stdin` | Standard input stream. |
| 14 | `process.stdout` | Standard output stream. |
| 15 | `process.stderr` | Standard error stream. |
| 16 | `SIGINT` | Interrupt signal, often Ctrl+C. |
| 17 | `process.on("SIGTERM", handler)` | Handles termination signal. |
| 18 | `.env file` | Common local environment file convention. |
| 19 | `dotenv` | Popular library for loading `.env` files. |
| 20 | `validate env at startup` | Fail fast when config is missing. |
| 21 | `process.env.NODE_ENV ?? "development"` | Reads an environment with a default. |
| 22 | `process.chdir(path)` | Changes the current working directory. |
| 23 | `process.hrtime.bigint()` | Gets high-resolution time for measurement. |
| 24 | `process.exit(1)` | Immediately exits with a failure code. |
| 25 | `process.kill(process.pid, "SIGTERM")` | Sends a signal to a process. |
| 26 | `process.on("exit", handler)` | Runs synchronous cleanup when process exits. |
| 27 | `process.stdin.setEncoding("utf8")` | Reads standard input as text. |
| 28 | `process.stdout.write("ok\\n")` | Writes without console formatting. |
| 29 | `process.env.PATH` | Reads the OS executable search path. |
| 30 | `parseArgs` | Node utility API for parsing CLI arguments. |

## Interview Questions

**Q1. What is `process.argv`?**  
It is an array containing the Node executable path, script path, and command-line arguments.

**Q2. Why are environment variables common in Node apps?**  
They configure apps differently across environments without changing code, especially for ports, secrets, and service URLs.

**Q3. Why should environment variables be validated at startup?**  
They are strings or undefined, so missing or malformed config should fail fast before the app handles traffic.

**Q4. What is the difference between `process.exitCode = 1` and `process.exit(1)`?**  
`exitCode` marks the process as failed but lets pending work finish. `process.exit()` terminates immediately.

**Q5. What is a signal like `SIGTERM` used for?**  
It tells a process to shut down, often from process managers, containers, or orchestration platforms.
