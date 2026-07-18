# 04 - Files, Paths, and Directories

Node's `node:fs` module works with files. Prefer promise-based APIs in modern code:

```js
import { readFile, writeFile } from "node:fs/promises";

const text = await readFile("notes.txt", "utf8");
await writeFile("copy.txt", text);
```

Use `node:path` for paths instead of string concatenation:

```js
import path from "node:path";

const filePath = path.join("data", "tasks.json");
```

File APIs can fail for many normal reasons: missing files, permission issues, invalid paths, locked files, and disk problems. Handle expected failures intentionally.

## Exercises

1. Read a text file.
2. Write a JSON file.
3. Create a directory.
4. Join paths with `path.join`.
5. Handle a missing file error.
