# 10 - Express APIs

Express is a popular minimalist web framework for Node.

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

Express adds routing, middleware, request helpers, response helpers, error middleware, and ecosystem conventions.

Keep route handlers thin. Move business logic into services so it can be tested without HTTP.

## Exercises

1. Create an Express app.
2. Add JSON body parsing.
3. Add CRUD routes.
4. Add error middleware.
5. Test routes with an HTTP client.
