# 06 - Async Patterns and the Event Loop

Node is built around asynchronous I/O. Most file, network, timer, and database work should not block the event loop.

Promise-based code:

```js
const text = await readFile("notes.txt", "utf8");
```

Run independent work concurrently:

```js
const [users, orders] = await Promise.all([
  loadUsers(),
  loadOrders()
]);
```

The event loop lets Node continue handling other work while async operations are pending. CPU-heavy work is different: it can block the event loop and make the process unresponsive.

For CPU-heavy tasks, consider worker threads, child processes, queues, or moving work to another service.

## Exercises

1. Write an async function.
2. Run two async operations sequentially.
3. Rewrite them with `Promise.all`.
4. Create a delay promise.
5. Observe how a CPU-heavy loop blocks timers.
