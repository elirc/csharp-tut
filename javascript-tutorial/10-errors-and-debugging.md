# 10 - Errors and Debugging

Errors are objects that describe failures. Throw errors when a function cannot complete its contract.

```js
function divide(a, b) {
  if (b === 0) {
    throw new Error("Cannot divide by zero");
  }
  return a / b;
}
```

Use `try`/`catch` when you can handle the failure meaningfully.

```js
try {
  const data = JSON.parse(text);
} catch (err) {
  console.error("Invalid JSON", err);
}
```

Debugging is a skill. Learn breakpoints, stepping, watch expressions, stack traces, and the browser/Node debugger. `console.log` is useful, but it should not be your only debugging tool.

## Exercises

1. Throw an error for invalid input.
2. Catch a JSON parsing error.
3. Set a breakpoint in your editor.
4. Read a stack trace and identify the failing line.
