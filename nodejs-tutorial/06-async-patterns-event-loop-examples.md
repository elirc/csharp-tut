# 06 - Async Patterns and the Event Loop Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `async function run() {}` | Declares async function. |
| 2 | `await promise` | Waits for promise result. |
| 3 | `Promise.resolve(value)` | Already-resolved promise. |
| 4 | `Promise.reject(error)` | Already-rejected promise. |
| 5 | `Promise.all(tasks)` | Waits for all promises. |
| 6 | `Promise.allSettled(tasks)` | Waits for every outcome. |
| 7 | `Promise.race(tasks)` | Settles with first settled promise. |
| 8 | `setTimeout(fn, ms)` | Schedules timer callback. |
| 9 | `setImmediate(fn)` | Schedules callback after current poll phase. |
| 10 | `process.nextTick(fn)` | Schedules high-priority microtask-like callback. |
| 11 | `queueMicrotask(fn)` | Schedules microtask. |
| 12 | `AbortController` | Cancels supported async work. |
| 13 | `AbortSignal.timeout(ms)` | Creates timeout signal in modern Node. |
| 14 | `event loop` | Coordinates async callbacks. |
| 15 | `non-blocking I/O` | Allows other work while waiting. |
| 16 | `CPU-bound work` | Can block the event loop. |
| 17 | `worker_threads` | Runs CPU work in separate threads. |
| 18 | `child_process` | Runs separate processes. |
| 19 | `backpressure` | Consumer controls producer speed. |
| 20 | `top-level await` | Available in ES modules. |
| 21 | `await timersPromises.setTimeout(1000)` | Promise-based timer from `node:timers/promises`. |
| 22 | `Promise.any(tasks)` | Resolves with the first fulfilled promise. |
| 23 | `for await (const item of asyncIterable)` | Consumes async iterable values. |
| 24 | `async generator` | Produces async values over time. |
| 25 | `setTimeout(fn, 0)` | Schedules a macrotask after current work. |
| 26 | `microtask queue` | Runs promise callbacks before many later event-loop phases. |
| 27 | `event-loop starvation` | Too much high-priority work delays other callbacks. |
| 28 | `p-limit` | Common package for limiting async concurrency. |
| 29 | `workerData` | Input passed into a worker thread. |
| 30 | `parentPort.postMessage()` | Sends data from a worker thread to parent. |

## Interview Questions

**Q1. What does non-blocking I/O mean in Node.js?**  
It means Node can start an I/O operation and continue handling other work while waiting for that operation to finish.

**Q2. Does `await` block the entire Node process?**  
No. It pauses the current async function, but the event loop can continue processing other work.

**Q3. When should you use `Promise.all`?**  
Use it when independent async operations can run concurrently and all results are needed.

**Q4. Why is CPU-heavy work a problem in Node?**  
CPU-heavy JavaScript runs on the main thread by default and can block the event loop, delaying requests, timers, and callbacks.

**Q5. How can you handle CPU-heavy work in Node?**  
Use worker threads, child processes, queues, native services, or move the work outside the request path.
