# 09 - HTTP Servers and Routing

Node has a built-in HTTP server module.

```js
import http from "node:http";

const server = http.createServer((req, res) => {
  res.writeHead(200, { "Content-Type": "application/json" });
  res.end(JSON.stringify({ ok: true }));
});

server.listen(3000);
```

Built-in HTTP is useful to understand because frameworks build on the same ideas: request, response, method, URL, headers, body, status codes, and streams.

For real apps, most teams use frameworks such as Express, Fastify, Hono, NestJS, or framework-level platforms.

## Exercises

1. Create a basic HTTP server.
2. Return JSON.
3. Route by method and URL.
4. Read a request body.
5. Return 404 for unknown paths.
