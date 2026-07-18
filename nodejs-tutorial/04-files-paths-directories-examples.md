# 04 - Files, Paths, and Directories Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `readFile(path, "utf8")` | Reads text file. |
| 2 | `writeFile(path, text)` | Writes text file. |
| 3 | `appendFile(path, text)` | Appends text. |
| 4 | `mkdir(path, { recursive: true })` | Creates directory tree. |
| 5 | `rm(path)` | Removes file or folder. |
| 6 | `rename(oldPath, newPath)` | Moves or renames. |
| 7 | `copyFile(source, target)` | Copies a file. |
| 8 | `stat(path)` | Reads file metadata. |
| 9 | `readdir(path)` | Lists directory entries. |
| 10 | `path.join("data", "file.txt")` | Builds platform-safe path. |
| 11 | `path.resolve("data")` | Creates absolute path. |
| 12 | `path.extname(file)` | Gets extension. |
| 13 | `path.basename(file)` | Gets filename. |
| 14 | `path.dirname(file)` | Gets directory path. |
| 15 | `URL import.meta.url` | Useful for module-relative paths. |
| 16 | `JSON.parse(text)` | Parses stored JSON. |
| 17 | `JSON.stringify(data, null, 2)` | Pretty JSON output. |
| 18 | `ENOENT` | Common missing file error code. |
| 19 | `EACCES` | Common permission error code. |
| 20 | `avoid sync I/O in servers` | Synchronous file work blocks the event loop. |
| 21 | `access(path)` | Checks whether a path can be accessed. |
| 22 | `open(path, "r")` | Opens a file handle for reading. |
| 23 | `filehandle.close()` | Closes an opened file handle. |
| 24 | `mkdtemp(prefix)` | Creates a temporary directory. |
| 25 | `cp(source, target, { recursive: true })` | Copies files or directories recursively. |
| 26 | `watch(path, listener)` | Watches for file system changes. |
| 27 | `path.parse(file)` | Splits a path into parts. |
| 28 | `path.format(parts)` | Builds a path from parsed parts. |
| 29 | `path.isAbsolute(file)` | Checks whether a path is absolute. |
| 30 | `new URL("./data.json", import.meta.url)` | Builds a module-relative file URL. |

## Interview Questions

**Q1. Why should you use `node:path` instead of string concatenation for paths?**  
It handles platform-specific separators and path rules correctly.

**Q2. Why avoid synchronous file APIs in a web server?**  
They block the event loop, preventing the process from handling other requests while the operation runs.

**Q3. What does `ENOENT` usually mean?**  
It means a file or directory was not found.

**Q4. When would you use streams instead of `readFile`?**  
Use streams for large files or continuous data so the app does not load everything into memory.

**Q5. Why is JSON file storage acceptable for learning but risky for production?**  
It lacks robust concurrency, transactions, indexing, durability guarantees, and operational tooling compared with databases.
