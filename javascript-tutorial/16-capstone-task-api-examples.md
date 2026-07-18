# 16 - Capstone: A Small Task API Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `GET /api/tasks` | Lists tasks. |
| 2 | `GET /api/tasks/:id` | Gets one task. |
| 3 | `POST /api/tasks` | Creates a task. |
| 4 | `PUT /api/tasks/:id` | Replaces task fields. |
| 5 | `PATCH /api/tasks/:id/complete` | Updates one behavior-specific field. |
| 6 | `DELETE /api/tasks/:id` | Deletes a task. |
| 7 | `crypto.randomUUID()` | Creates task ids. |
| 8 | `{ title, completed: false }` | Basic task shape. |
| 9 | `if (!title) return res.status(400)` | Validates required input. |
| 10 | `const task = service.find(id)` | Keeps lookup logic outside the route. |
| 11 | `res.status(404).json({ error })` | Returns not found clearly. |
| 12 | `res.status(201).json(task)` | Returns created data. |
| 13 | `app.use(errorHandler)` | Centralizes unexpected error handling. |
| 14 | `structuredClone(task)` | Copies data for safer boundaries. |
| 15 | `tasks.filter((t) => !t.deletedAt)` | Implements soft delete queries. |
| 16 | `tasks.sort((a, b) => a.createdAt.localeCompare(b.createdAt))` | Sorts by timestamp text. |
| 17 | `tasks.slice(skip, skip + take)` | Implements simple paging. |
| 18 | `await request(app).get("/api/tasks")` | Tests an endpoint with Supertest. |
| 19 | `expect(response.status).toBe(200)` | Asserts HTTP status in a test. |
| 20 | `expect(response.body).toEqual(expect.any(Array))` | Asserts response shape. |
