# 07 - Buffers and Streams

Buffers represent binary data.

```js
const buffer = Buffer.from("hello");
console.log(buffer.toString("utf8"));
```

Streams process data in chunks instead of loading everything into memory. They are important for files, HTTP requests, responses, compression, uploads, downloads, and large data processing.

```js
import { createReadStream, createWriteStream } from "node:fs";
import { pipeline } from "node:stream/promises";

await pipeline(
  createReadStream("input.txt"),
  createWriteStream("output.txt")
);
```

Use `pipeline` instead of manually wiring stream events when possible because it handles errors and cleanup more safely.

## Exercises

1. Create a buffer from text.
2. Read a file with a stream.
3. Write a file with a stream.
4. Pipe one stream to another.
5. Handle stream errors.
