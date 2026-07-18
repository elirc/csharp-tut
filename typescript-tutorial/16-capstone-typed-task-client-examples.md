# 16 - Capstone: Typed Task Client Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `type TaskId = string` | Meaningful alias for ids. |
| 2 | `type TaskStatus = "open" \| "completed"` | Status union. |
| 3 | `type Task = { id: TaskId; title: string }` | Main domain type. |
| 4 | `type CreateTaskInput = Pick<Task, "title">` | Input type derived from domain type. |
| 5 | `type UpdateTaskInput = Partial<Pick<Task, "title" \| "status">>` | Partial update shape. |
| 6 | `Promise<Task[]>` | Async API result. |
| 7 | `fetchJson<T>()` | Generic fetch helper. |
| 8 | `unknown response data` | Treat API data as untrusted. |
| 9 | `isTask(value)` | Runtime type guard. |
| 10 | `isTaskArray(value)` | Validates arrays of tasks. |
| 11 | `Result<T>` | Typed success/failure wrapper. |
| 12 | `{ ok: true; value: T }` | Success result variant. |
| 13 | `{ ok: false; error: string }` | Failure result variant. |
| 14 | `switch (result.ok)` | Narrows result variants. |
| 15 | `HttpError` | Custom error for non-success responses. |
| 16 | `NotFoundError` | Specific error for 404. |
| 17 | `AbortSignal` | Supports request cancellation. |
| 18 | `readonly Task[]` | Prevents client consumers from mutating returned array. |
| 19 | `test invalid JSON` | Verifies validation catches bad data. |
| 20 | `document public API` | README should show typed client usage. |
