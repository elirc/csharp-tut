# 06 - Effects and Data Fetching Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `useEffect(() => {}, [])` | Runs after initial render. |
| 2 | `useEffect(() => {}, [id])` | Runs when `id` changes. |
| 3 | `return () => cleanup()` | Cleans up an effect. |
| 4 | `setInterval(tick, 1000)` | Starts a timer. |
| 5 | `clearInterval(id)` | Cleans up a timer. |
| 6 | `window.addEventListener("resize", onResize)` | Subscribes to a browser event. |
| 7 | `window.removeEventListener("resize", onResize)` | Unsubscribes during cleanup. |
| 8 | `fetch("/api/tasks")` | Starts a network request. |
| 9 | `await response.json()` | Parses JSON response data. |
| 10 | `setLoading(true)` | Tracks loading state. |
| 11 | `setError(message)` | Tracks failure state. |
| 12 | `AbortController` | Cancels fetch when appropriate. |
| 13 | `controller.abort()` | Aborts an in-flight request. |
| 14 | `ignore = true` | Prevents stale async updates after cleanup. |
| 15 | `document.title = title` | Synchronizes the page title. |
| 16 | `localStorage.setItem(key, value)` | Synchronizes browser storage. |
| 17 | `const filtered = items.filter(...)` | Calculate during render instead of effect. |
| 18 | `useEffect` without dependency array | Runs after every render. |
| 19 | `exhaustive-deps` | Lint rule that checks effect dependencies. |
| 20 | `custom data hook` | Moves repeated effect logic into a reusable hook. |
