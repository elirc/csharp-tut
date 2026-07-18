# 09 - Async, Promises, and Fetch Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `Promise.resolve(42)` | Creates an already-resolved promise. |
| 2 | `Promise.reject(new Error("No"))` | Creates an already-rejected promise. |
| 3 | `await promise` | Waits for a promise inside an async function. |
| 4 | `async function load() {}` | Declares an async function. |
| 5 | `const load = async () => {};` | Declares an async arrow function. |
| 6 | `try { await work(); } catch (err) {}` | Handles async failure. |
| 7 | `Promise.all([a(), b()])` | Waits for all promises. |
| 8 | `Promise.race([a(), b()])` | Resolves or rejects with the first settled promise. |
| 9 | `Promise.allSettled(promises)` | Waits for every promise outcome. |
| 10 | `fetch(url)` | Sends an HTTP request in browser or modern Node. |
| 11 | `response.ok` | Checks for a 2xx HTTP response. |
| 12 | `response.status` | Reads the HTTP status code. |
| 13 | `await response.json()` | Parses JSON response content. |
| 14 | `await response.text()` | Reads response content as text. |
| 15 | `fetch(url, { method: "POST" })` | Sends a POST request. |
| 16 | `JSON.stringify(body)` | Converts a request body to JSON text. |
| 17 | `headers: { "Content-Type": "application/json" }` | Tells the server JSON is being sent. |
| 18 | `AbortController` | Cancels fetch and other async operations. |
| 19 | `setTimeout(resolve, 1000)` | Common way to build a delay promise. |
| 20 | `queueMicrotask(fn)` | Schedules work in the microtask queue. |
