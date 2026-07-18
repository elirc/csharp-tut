# 00 - Runtime and Setup

Node.js is a JavaScript runtime built for running JavaScript outside the browser. It is commonly used for command-line tools, backend APIs, workers, build tools, and automation.

Check your install:

```bash
node --version
npm --version
```

Run a file:

```bash
node src/index.js
```

Node has built-in modules such as `node:fs`, `node:path`, `node:http`, `node:stream`, `node:test`, and `node:process`. It also has package tooling through npm.

Modern Node can run ES modules, use top-level `await` in modules, expose browser-like APIs such as `fetch`, and run tests with the built-in test runner.

## Exercises

1. Print your Node version.
2. Create and run `src/index.js`.
3. Import a built-in module.
4. Run a script from `package.json`.
5. Explain what Node provides that the browser does not.
