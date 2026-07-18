# 13 - Data, JSON, and Storage

JSON is the standard data format for JavaScript applications. JavaScript objects and JSON look similar, but JSON is text and has stricter rules.

```js
const text = JSON.stringify({ name: "Ada" });
const value = JSON.parse(text);
```

For small projects, you can store data in memory or in JSON files. For real applications, use a database. JavaScript teams commonly use PostgreSQL, MySQL, SQLite, MongoDB, Redis, and hosted storage services.

The important skill is modeling data deliberately. Know the difference between data transfer objects, domain objects, persistence models, and user-facing response shapes.

## Exercises

1. Serialize an object to JSON.
2. Parse JSON and handle invalid text.
3. Save an array to a file.
4. Model a task with id, title, completion status, and timestamps.
