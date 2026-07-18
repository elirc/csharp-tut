# 12 - HTTP APIs with Express

Express is a small, common Node.js framework for building HTTP APIs.

```bash
npm install express
```

```js
import express from "express";

const app = express();
app.use(express.json());

app.get("/api/health", (req, res) => {
  res.json({ ok: true });
});

app.listen(3000);
```

Routes map HTTP methods and paths to handlers. Middleware runs before handlers and can parse JSON, log requests, authenticate users, or handle errors.

Keep route handlers thin as the app grows. Move business logic into functions or services so it can be tested without HTTP.

## Exercises

1. Create a health endpoint.
2. Add a GET endpoint that returns an array.
3. Add a POST endpoint that reads `req.body`.
4. Add error-handling middleware.
