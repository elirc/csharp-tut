# 05 - Errors and Debugging Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `throw new Error("Failed")` | Throws a general error. |
| 2 | `try { await work(); } catch (error) {}` | Catches async failure. |
| 3 | `error.message` | Human-readable error message. |
| 4 | `error.stack` | Stack trace. |
| 5 | `error.code` | Node system errors often include a code. |
| 6 | `ENOENT` | Missing file or directory. |
| 7 | `EACCES` | Permission denied. |
| 8 | `ECONNREFUSED` | Connection refused. |
| 9 | `ETIMEDOUT` | Operation timed out. |
| 10 | `process.on("uncaughtException")` | Last-resort uncaught error hook. |
| 11 | `process.on("unhandledRejection")` | Last-resort unhandled promise hook. |
| 12 | `node --inspect app.js` | Starts inspector debugging. |
| 13 | `debugger;` | Breakpoint statement. |
| 14 | `console.trace()` | Prints current stack. |
| 15 | `console.time("work")` | Starts timing. |
| 16 | `console.timeEnd("work")` | Ends timing. |
| 17 | `assert.ok(value)` | Fails fast in tests or scripts. |
| 18 | `cause` | Error option for wrapping causes. |
| 19 | `AggregateError` | Represents multiple errors. |
| 20 | `fail fast` | Stop startup when required config is invalid. |
| 21 | `new Error("Failed", { cause })` | Wraps a lower-level error while preserving context. |
| 22 | `Error.captureStackTrace` | Customizes stack trace creation in V8 environments. |
| 23 | `node --trace-uncaught app.js` | Prints more detail for uncaught exceptions. |
| 24 | `node --trace-deprecation app.js` | Shows stack traces for deprecation warnings. |
| 25 | `process.emitWarning("message")` | Emits a process warning. |
| 26 | `assert.rejects(asyncFn)` | Verifies an async function rejects. |
| 27 | `try/finally` | Ensures cleanup runs after success or failure. |
| 28 | `custom Error subclass` | Creates domain-specific error types. |
| 29 | `operational error` | Expected runtime failure such as missing file or refused connection. |
| 30 | `programmer error` | Bug in code, such as invalid assumptions or bad arguments. |

## Interview Questions

**Q1. How do you catch errors from an awaited promise?**  
Wrap the `await` in `try`/`catch`.

**Q2. Why is an unhandled promise rejection dangerous?**  
It means async failure was not handled, which can crash the process or leave the app in an unknown state.

**Q3. What is useful about `error.code` in Node?**  
Node system errors often include stable codes like `ENOENT` or `ECONNREFUSED`, which are better to branch on than message text.

**Q4. What is the difference between an operational error and a programmer error?**  
Operational errors are expected runtime failures. Programmer errors are bugs that should usually be fixed rather than handled as normal flow.

**Q5. How do you debug a Node process with breakpoints?**  
Start it with `node --inspect` or use your editor's Node debugger configuration.
