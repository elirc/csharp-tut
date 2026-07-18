# 00 - Runtime and Setup Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `node --version` | Shows installed Node.js version. |
| 2 | `npm --version` | Shows installed npm version. |
| 3 | `node src/index.js` | Runs a JavaScript file. |
| 4 | `node -e "console.log(1)"` | Runs inline JavaScript. |
| 5 | `node --watch src/index.js` | Reruns when files change in supported Node versions. |
| 6 | `globalThis` | Global object available across environments. |
| 7 | `process` | Node process information and controls. |
| 8 | `Buffer` | Node binary data type. |
| 9 | `fetch` | HTTP client API in modern Node. |
| 10 | `node:fs` | Built-in file system module. |
| 11 | `node:path` | Built-in path utility module. |
| 12 | `node:http` | Built-in HTTP module. |
| 13 | `node:test` | Built-in test runner module. |
| 14 | `node --help` | Shows Node CLI options. |
| 15 | `node --inspect app.js` | Starts Node with debugger support. |
| 16 | `LTS` | Recommended stable release line for most apps. |
| 17 | `REPL` | Interactive Node prompt. |
| 18 | `node` | Starts the REPL when run without a file. |
| 19 | `Ctrl+C` | Stops a running Node process in the terminal. |
| 20 | `runtime` | Environment that executes JavaScript code. |
| 21 | `node --eval "console.log(process.version)"` | Runs a one-off script without creating a file. |
| 22 | `node --print "1 + 2"` | Evaluates an expression and prints the result. |
| 23 | `process.release.name` | Confirms the runtime release name, usually `node`. |
| 24 | `process.execPath` | Shows the absolute path to the Node executable. |
| 25 | `process.execArgv` | Reads Node-specific CLI flags passed to the process. |
| 26 | `globalThis.fetch` | Checks whether the runtime exposes `fetch`. |
| 27 | `node --disable-warning=ExperimentalWarning app.js` | Suppresses a specific warning category when appropriate. |
| 28 | `node --trace-warnings app.js` | Shows stack traces for process warnings. |
| 29 | `node -p "process.versions.v8"` | Prints the V8 engine version used by Node. |
| 30 | `nvm use --lts` | Switches Node versions when using a version manager. |

## Interview Questions

**Q1. What is Node.js?**  
Node.js is a JavaScript runtime that lets JavaScript run outside the browser, commonly for servers, CLIs, automation, and tooling.

**Q2. What is the difference between Node.js and npm?**  
Node.js runs JavaScript. npm manages packages, scripts, and dependency installation for Node projects.

**Q3. Why do teams care about Node LTS versions?**  
LTS versions are stable release lines that receive longer support, making them safer for production applications.

**Q4. What does the Node runtime provide that browser JavaScript does not?**  
Node provides server/runtime APIs such as file system access, process control, streams, TCP/HTTP servers, and package-based modules.

**Q5. Why might code work in Node but fail in the browser?**  
The language is shared, but the runtime APIs differ. For example, `process` and `node:fs` exist in Node, while `document` exists in browsers.
