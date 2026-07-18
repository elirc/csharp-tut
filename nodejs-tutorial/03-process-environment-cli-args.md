# 03 - Process, Environment, and CLI Args

Node exposes the running process through `process`.

```js
console.log(process.cwd());
console.log(process.argv);
console.log(process.env.NODE_ENV);
```

Environment variables configure apps without changing code. They are strings or undefined.

```js
const port = Number(process.env.PORT ?? 3000);
```

Command-line arguments are useful for scripts and CLIs:

```js
const args = process.argv.slice(2);
```

Processes can signal success or failure with exit codes. Prefer setting `process.exitCode` over calling `process.exit()` abruptly when cleanup may still need to run.

## Exercises

1. Print the current working directory.
2. Read command-line arguments.
3. Read a `PORT` environment variable.
4. Validate required environment variables.
5. Set a non-zero exit code on failure.
