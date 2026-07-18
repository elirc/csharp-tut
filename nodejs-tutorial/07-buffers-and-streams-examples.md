# 07 - Buffers and Streams Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `Buffer.from("hello")` | Creates binary data from text. |
| 2 | `buffer.toString("utf8")` | Converts buffer to text. |
| 3 | `Buffer.alloc(1024)` | Allocates zero-filled buffer. |
| 4 | `createReadStream(path)` | Creates readable file stream. |
| 5 | `createWriteStream(path)` | Creates writable file stream. |
| 6 | `readable.pipe(writable)` | Pipes data between streams. |
| 7 | `pipeline(input, output)` | Promise-based safe stream pipeline. |
| 8 | `Readable` | Stream that produces data. |
| 9 | `Writable` | Stream that consumes data. |
| 10 | `Duplex` | Stream that reads and writes. |
| 11 | `Transform` | Stream that changes data. |
| 12 | `data event` | Chunk available from stream. |
| 13 | `end event` | Readable stream finished. |
| 14 | `error event` | Stream failure event. |
| 15 | `backpressure` | Prevents producer overwhelming consumer. |
| 16 | `highWaterMark` | Internal buffer threshold. |
| 17 | `process.stdin` | Readable stream. |
| 18 | `process.stdout` | Writable stream. |
| 19 | `HTTP request stream` | Incoming request body is stream-like. |
| 20 | `large files` | Streams avoid loading all data at once. |
| 21 | `Buffer.byteLength(text)` | Counts bytes needed to store text. |
| 22 | `buffer.subarray(start, end)` | Creates a view over part of a buffer. |
| 23 | `Readable.from(items)` | Creates a readable stream from iterable data. |
| 24 | `finished(stream)` | Promise helper that resolves when a stream ends or errors. |
| 25 | `stream/promises` | Promise-based stream helpers. |
| 26 | `zlib.createGzip()` | Transform stream for compression. |
| 27 | `request.pipe(response)` | Simple stream piping pattern. |
| 28 | `writable.write(chunk)` | Writes a chunk to a writable stream. |
| 29 | `writable.end()` | Signals no more data will be written. |
| 30 | `objectMode` | Stream mode for JavaScript objects instead of buffers/strings. |

## Interview Questions

**Q1. What is a Buffer in Node.js?**  
A Buffer is a binary data container used for bytes, files, network data, and streams.

**Q2. Why are streams useful?**  
They process data in chunks, which avoids loading large files or responses entirely into memory.

**Q3. What is backpressure?**  
Backpressure is flow control where a slower consumer signals a faster producer to slow down.

**Q4. Why prefer `pipeline` over manually calling `.pipe()` in important code?**  
`pipeline` handles errors and cleanup more reliably and can be awaited through the promises API.

**Q5. Name the four common stream types.**  
Readable, Writable, Duplex, and Transform.
