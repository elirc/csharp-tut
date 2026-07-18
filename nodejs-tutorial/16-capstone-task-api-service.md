# 16 - Capstone: Task API Service

Build a small Node task API service that uses the runtime, not just JavaScript syntax.

Suggested features:

```text
GET    /api/tasks
GET    /api/tasks/:id
POST   /api/tasks
PUT    /api/tasks/:id
PATCH  /api/tasks/:id/complete
DELETE /api/tasks/:id
GET    /health
```

Suggested structure:

```text
src/server.js
src/app.js
src/routes/tasks.js
src/services/taskService.js
src/storage/taskStore.js
src/middleware/errorHandler.js
test/
```

Start with in-memory storage. Then move to JSON file storage or a database. Add validation, structured errors, tests, logging, graceful shutdown, and deployment notes.

The goal is a service shape that resembles real Node backend work.

## Exercises

1. Build all task endpoints.
2. Move business logic out of routes.
3. Add request validation.
4. Add built-in `node:test` tests.
5. Add graceful shutdown.
6. Write run/test/deploy instructions.
