# 14 - API Integration and Server State Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `fetch("/api/tasks")` | Requests server data. |
| 2 | `response.ok` | Checks HTTP success. |
| 3 | `await response.json()` | Parses response data. |
| 4 | `setLoading(true)` | Tracks request progress. |
| 5 | `setError(error.message)` | Tracks request failure. |
| 6 | `setData(data)` | Stores loaded data. |
| 7 | `POST /api/tasks` | Creates data on the server. |
| 8 | `JSON.stringify(task)` | Sends JSON request body. |
| 9 | `Content-Type application/json` | Identifies request body format. |
| 10 | `AbortController` | Cancels in-flight requests. |
| 11 | `optimistic update` | Updates UI before server confirmation. |
| 12 | `rollback` | Reverts optimistic UI after failure. |
| 13 | `refetch` | Reloads server data. |
| 14 | `cache key` | Identifies cached server data. |
| 15 | `stale data` | Cached data that may need refreshing. |
| 16 | `TanStack Query` | Popular server-state library. |
| 17 | `mutation` | Server-changing operation. |
| 18 | `loading state` | UI while request is pending. |
| 19 | `empty state` | UI when request succeeds with no items. |
| 20 | `error state` | UI when request fails. |
