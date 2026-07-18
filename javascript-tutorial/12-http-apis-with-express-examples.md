# 12 - HTTP APIs with Express Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `import express from "express";` | Imports Express. |
| 2 | `const app = express();` | Creates an app instance. |
| 3 | `app.use(express.json());` | Parses JSON request bodies. |
| 4 | `app.get("/users", handler)` | Registers a GET route. |
| 5 | `app.post("/users", handler)` | Registers a POST route. |
| 6 | `app.put("/users/:id", handler)` | Registers a PUT route with a route parameter. |
| 7 | `app.delete("/users/:id", handler)` | Registers a DELETE route. |
| 8 | `req.params.id` | Reads a route parameter. |
| 9 | `req.query.page` | Reads a query string value. |
| 10 | `req.body` | Reads parsed request body data. |
| 11 | `res.status(201).json(user)` | Sends a JSON response with status. |
| 12 | `res.sendStatus(204)` | Sends a status with no response body. |
| 13 | `next(err)` | Passes an error to error middleware. |
| 14 | `(err, req, res, next) => {}` | Express error middleware signature. |
| 15 | `app.use(logger)` | Registers normal middleware. |
| 16 | `app.listen(3000)` | Starts the HTTP server. |
| 17 | `req.headers.authorization` | Reads the authorization header. |
| 18 | `res.set("Cache-Control", "no-store")` | Sets a response header. |
| 19 | `app.Router()` | Creates modular route groups. |
| 20 | `return res.status(404).json({ error: "Not found" })` | Returns a clear not-found response. |
