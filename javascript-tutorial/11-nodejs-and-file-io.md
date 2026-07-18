# 11 - Node.js and File I/O

Node.js lets JavaScript work with the operating system. Use `node:` imports for built-in modules.

```js
import { readFile, writeFile } from "node:fs/promises";

const text = await readFile("notes.txt", "utf8");
await writeFile("copy.txt", text);
```

Prefer promise-based APIs for new code. They compose naturally with `async` and `await`.

Paths should be built with `node:path`, not string concatenation:

```js
import path from "node:path";

const filePath = path.join("data", "settings.json");
```

Node also exposes process information, environment variables, streams, and command-line arguments. These are essential for scripts, CLIs, and backend services.

## Exercises

1. Read a text file.
2. Write a JSON file.
3. Build a path with `path.join`.
4. Read an environment variable.
