# 10 - Errors and Debugging Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `throw new Error("Failed")` | Throws a general error. |
| 2 | `new TypeError("Wrong type")` | Represents a type-related failure. |
| 3 | `try { risky(); } catch (err) {}` | Catches a thrown error. |
| 4 | `finally { cleanup(); }` | Runs cleanup after success or failure. |
| 5 | `err.message` | Reads an error message. |
| 6 | `err.stack` | Reads the stack trace. |
| 7 | `console.error(err)` | Logs an error. |
| 8 | `console.trace()` | Prints the current stack trace. |
| 9 | `debugger;` | Triggers a breakpoint. |
| 10 | `JSON.parse(text)` | Throws when JSON is invalid. |
| 11 | `Number.isNaN(value)` | Detects invalid numeric results. |
| 12 | `if (!response.ok) throw new Error()` | Converts HTTP failure to an exception. |
| 13 | `class AppError extends Error {}` | Creates a custom error type. |
| 14 | `err instanceof AppError` | Checks a custom error type. |
| 15 | `process.exitCode = 1` | Marks a Node process as failed. |
| 16 | `node --inspect app.js` | Starts Node with debugger support. |
| 17 | `console.time("work")` | Starts a simple timer. |
| 18 | `console.timeEnd("work")` | Ends and prints the timer. |
| 19 | `assert(condition)` | Fails fast when an assumption is false. |
| 20 | `throw new AggregateError(errors)` | Groups multiple errors together. |
