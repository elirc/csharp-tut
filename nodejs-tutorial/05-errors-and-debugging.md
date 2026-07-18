# 05 - Errors and Debugging

Node errors are usually normal JavaScript `Error` objects, sometimes with extra properties such as `code`.

```js
try {
  const text = await readFile("missing.txt", "utf8");
} catch (error) {
  if (error.code === "ENOENT") {
    console.error("File not found");
  } else {
    throw error;
  }
}
```

Async errors are caught with `try`/`catch` around `await`, `.catch()` on promises, or error events for streams and emitters.

Debug with stack traces, `console.error`, the inspector, breakpoints, and tests. Learn to read the first useful line in a stack trace.

## Exercises

1. Throw and catch an error.
2. Catch a rejected promise.
3. Handle a missing file error by code.
4. Run Node with `--inspect`.
5. Add a breakpoint in your editor.
