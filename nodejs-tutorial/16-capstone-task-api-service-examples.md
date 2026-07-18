# 16 - Capstone: Task API Service Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `GET /api/tasks` | List tasks. |
| 2 | `GET /api/tasks/:id` | Get one task. |
| 3 | `POST /api/tasks` | Create task. |
| 4 | `PUT /api/tasks/:id` | Replace task. |
| 5 | `PATCH /api/tasks/:id/complete` | Complete task. |
| 6 | `DELETE /api/tasks/:id` | Delete task. |
| 7 | `GET /health` | Health endpoint. |
| 8 | `taskService.create()` | Business logic boundary. |
| 9 | `taskStore.save()` | Persistence boundary. |
| 10 | `validateCreateTask()` | Input validation boundary. |
| 11 | `errorHandler` | Centralized error response middleware. |
| 12 | `crypto.randomUUID()` | Creates task ids. |
| 13 | `createdAt` | Creation timestamp. |
| 14 | `completedAt` | Completion timestamp. |
| 15 | `node --test` | Test runner command. |
| 16 | `supertest app` | HTTP integration testing helper. |
| 17 | `pino logger` | Structured logging option. |
| 18 | `SIGTERM handler` | Graceful shutdown support. |
| 19 | `Dockerfile` | Deployment packaging. |
| 20 | `README runbook` | Documents run, test, and deploy steps. |
| 21 | `GET /api/tasks?status=open` | Filters tasks by query string. |
| 22 | `GET /api/tasks?page=1&pageSize=20` | Adds basic pagination. |
| 23 | `409 Conflict` | Useful when an operation conflicts with current state. |
| 24 | `400 validation error` | Response for invalid request body. |
| 25 | `404 task not found` | Response when requested task does not exist. |
| 26 | `service unit tests` | Tests business logic without HTTP. |
| 27 | `route integration tests` | Tests HTTP behavior through the app. |
| 28 | `storage adapter` | Swappable persistence implementation. |
| 29 | `config module` | Validates and exposes environment configuration. |
| 30 | `operational checklist` | Captures run, test, logs, health, and shutdown behavior. |

## Interview Questions

**Q1. How would you structure a small Node API project?**  
Separate server startup, app configuration, routes, services, storage/data access, middleware, and tests.

**Q2. Why move business logic out of Express route handlers?**  
It makes logic easier to test, reuse, and maintain without depending on HTTP request/response objects.

**Q3. What status code should invalid input usually return?**  
Usually `400 Bad Request`, often with details explaining validation failures.

**Q4. What should the capstone demonstrate to an interviewer?**  
It should show routing, validation, error handling, persistence boundaries, tests, logging, configuration, and operational awareness.

**Q5. What is a good next improvement after an in-memory task API?**  
Add real persistence, runtime validation, authentication, structured logging, integration tests, and deployment documentation.
