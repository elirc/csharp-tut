# 13 - Security and Validation

Node services sit at boundaries: HTTP requests, files, databases, environment variables, queues, and third-party APIs. Treat boundary data as untrusted.

Validate inputs:

```js
function requireTitle(value) {
  if (typeof value !== "string" || value.trim() === "") {
    throw new Error("Title is required");
  }
  return value.trim();
}
```

Security basics:

- Do not store secrets in source control.
- Validate request bodies and query strings.
- Use parameterized database queries.
- Avoid logging secrets or tokens.
- Set safe HTTP headers.
- Rate limit sensitive endpoints.
- Keep dependencies updated.
- Understand authentication and authorization.

## Exercises

1. Validate a POST body.
2. Reject unknown fields.
3. Add a safe error response.
4. Check dependencies with `npm audit`.
5. Explain authentication versus authorization.
