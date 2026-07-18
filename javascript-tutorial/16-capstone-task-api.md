# 16 - Capstone: A Small Task API

The capstone is a small Express API for tasks. It combines modules, arrays, objects, async code, validation, error handling, and tests.

Endpoints:

```text
GET    /api/tasks
GET    /api/tasks/:id
POST   /api/tasks
PUT    /api/tasks/:id
PATCH  /api/tasks/:id/complete
DELETE /api/tasks/:id
```

Start simple with in-memory storage:

```js
const tasks = [];
```

Then separate responsibilities:

- `server.js` starts the app.
- `app.js` configures middleware and routes.
- `routes/tasks.js` defines HTTP routes.
- `services/taskService.js` owns task operations.
- `tests/` verifies behavior.

The goal is not to build a production task tracker. The goal is to practice moving through layers without losing track of behavior. Every endpoint should validate input, return appropriate status codes, and be covered by at least one test.

## Exercises

1. Build all six endpoints with in-memory data.
2. Move business logic out of route handlers.
3. Add tests for create, update, not found, and validation failure.
4. Replace in-memory storage with a JSON file or database.
