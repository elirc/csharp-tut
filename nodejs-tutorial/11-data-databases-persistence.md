# 11 - Data, Databases, and Persistence

Node apps can store data in memory, files, SQL databases, document databases, caches, queues, and object storage.

Start simple for learning:

```js
const tasks = [];
```

Then move to JSON files:

```js
await writeFile("tasks.json", JSON.stringify(tasks, null, 2));
```

Real backend apps usually use a database. Common choices include PostgreSQL, MySQL, SQLite, MongoDB, and Redis.

The important skill is treating persistence as a boundary: validate input, model data deliberately, handle failures, use transactions when needed, and avoid leaking database-specific shapes everywhere.

## Exercises

1. Store tasks in memory.
2. Save tasks to a JSON file.
3. Load tasks at startup.
4. Add validation before saving.
5. Research a PostgreSQL driver or ORM.
