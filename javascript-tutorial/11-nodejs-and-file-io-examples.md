# 11 - Node.js and File I/O Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `import { readFile } from "node:fs/promises";` | Imports async file reading. |
| 2 | `await readFile(path, "utf8")` | Reads text from a file. |
| 3 | `await writeFile(path, text)` | Writes text to a file. |
| 4 | `await appendFile(path, line)` | Appends text to a file. |
| 5 | `await mkdir("data", { recursive: true })` | Creates a folder if needed. |
| 6 | `await rm(path)` | Removes a file. |
| 7 | `await stat(path)` | Reads file metadata. |
| 8 | `path.join("data", "file.txt")` | Builds a platform-safe path. |
| 9 | `path.extname(file)` | Reads a file extension. |
| 10 | `path.basename(file)` | Reads the file name. |
| 11 | `process.env.NODE_ENV` | Reads an environment variable. |
| 12 | `process.argv.slice(2)` | Reads CLI arguments. |
| 13 | `process.cwd()` | Reads the current working directory. |
| 14 | `Buffer.from("hello")` | Creates binary data. |
| 15 | `createReadStream(path)` | Reads a file as a stream. |
| 16 | `createWriteStream(path)` | Writes a file as a stream. |
| 17 | `await pipeline(input, output)` | Pipes streams with backpressure handling. |
| 18 | `JSON.parse(text)` | Parses file text as JSON. |
| 19 | `JSON.stringify(data, null, 2)` | Formats JSON for storage. |
| 20 | `import os from "node:os";` | Imports operating system helpers. |
