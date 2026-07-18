# 10 - Express APIs Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `import express from "express"` | Imports Express. |
| 2 | `const app = express()` | Creates app. |
| 3 | `app.use(express.json())` | Parses JSON body. |
| 4 | `app.get("/tasks", handler)` | GET route. |
| 5 | `app.post("/tasks", handler)` | POST route. |
| 6 | `app.put("/tasks/:id", handler)` | PUT route with param. |
| 7 | `app.patch("/tasks/:id", handler)` | PATCH route. |
| 8 | `app.delete("/tasks/:id", handler)` | DELETE route. |
| 9 | `req.params.id` | Route parameter. |
| 10 | `req.query.page` | Query parameter. |
| 11 | `req.body` | Parsed JSON body. |
| 12 | `res.status(201).json(task)` | Sends JSON with status. |
| 13 | `res.sendStatus(204)` | Sends no-content response. |
| 14 | `next(error)` | Passes error to middleware. |
| 15 | `(err, req, res, next)` | Error middleware signature. |
| 16 | `express.Router()` | Creates route group. |
| 17 | `middleware` | Function in request pipeline. |
| 18 | `auth middleware` | Checks authentication. |
| 19 | `validation middleware` | Rejects invalid input. |
| 20 | `thin route handler` | Delegates business logic to services. |
| 21 | `app.use("/api/tasks", taskRouter)` | Mounts a router under a path prefix. |
| 22 | `req.get("Authorization")` | Reads a request header. |
| 23 | `res.set("Cache-Control", "no-store")` | Sets a response header. |
| 24 | `res.locals` | Stores request-scoped data for later middleware. |
| 25 | `app.disable("x-powered-by")` | Hides Express header that reveals framework. |
| 26 | `express.urlencoded({ extended: false })` | Parses form-encoded bodies. |
| 27 | `app.param("id", handler)` | Runs logic for route parameters. |
| 28 | `asyncHandler(fn)` | Wrapper pattern for async route errors. |
| 29 | `res.location(url)` | Sets Location header. |
| 30 | `res.redirect(url)` | Sends redirect response. |

## Interview Questions

**Q1. What is middleware in Express?**  
Middleware is a function that runs during request processing and can read/modify request/response or pass control with `next`.

**Q2. Why call `app.use(express.json())`?**  
It parses JSON request bodies and makes the result available as `req.body`.

**Q3. What is Express error middleware?**  
A middleware function with four parameters: `(err, req, res, next)`, used for centralized error handling.

**Q4. Why keep route handlers thin?**  
Thin handlers are easier to test and maintain because business logic lives in services or modules outside HTTP plumbing.

**Q5. What is the difference between `req.params`, `req.query`, and `req.body`?**  
`params` comes from route placeholders, `query` from the URL query string, and `body` from parsed request content.
