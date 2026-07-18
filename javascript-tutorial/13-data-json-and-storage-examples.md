# 13 - Data, JSON, and Storage Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `JSON.stringify(user)` | Converts a value to JSON text. |
| 2 | `JSON.stringify(user, null, 2)` | Pretty-prints JSON. |
| 3 | `JSON.parse(text)` | Converts JSON text to a JavaScript value. |
| 4 | `try { JSON.parse(text); } catch {}` | Handles invalid JSON. |
| 5 | `Date.now()` | Gets a timestamp in milliseconds. |
| 6 | `new Date().toISOString()` | Creates a portable date string. |
| 7 | `crypto.randomUUID()` | Creates a unique id in modern runtimes. |
| 8 | `{ id, title, completed: false }` | Models a simple task. |
| 9 | `tasks.find((t) => t.id === id)` | Looks up an in-memory record. |
| 10 | `tasks.filter((t) => !t.completed)` | Queries in-memory records. |
| 11 | `tasks.map((t) => ({ ...t, done: true }))` | Creates updated copies. |
| 12 | `localStorage.setItem("tasks", json)` | Stores browser data. |
| 13 | `localStorage.getItem("tasks")` | Reads browser data. |
| 14 | `await writeFile("tasks.json", json)` | Stores data in a JSON file. |
| 15 | `await readFile("tasks.json", "utf8")` | Reads JSON file text. |
| 16 | `Number(req.params.id)` | Converts route ids to numbers. |
| 17 | `if (!title) throw new Error("Title required")` | Validates required data. |
| 18 | `createdAt` | Stores when a record was created. |
| 19 | `updatedAt` | Stores when a record was last changed. |
| 20 | `deletedAt` | Common field for soft delete. |
