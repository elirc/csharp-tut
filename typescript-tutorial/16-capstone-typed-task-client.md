# 16 - Capstone: Typed Task Client

Build a small typed client for a task API. This project should combine object types, unions, narrowing, generics, API boundaries, runtime validation, and tests.

Suggested API functions:

```ts
getTasks(): Promise<Task[]>
getTask(id: TaskId): Promise<Task>
createTask(input: CreateTaskInput): Promise<Task>
completeTask(id: TaskId): Promise<Task>
deleteTask(id: TaskId): Promise<void>
```

Suggested types:

```ts
type TaskId = string;

type TaskStatus = "open" | "completed";

type Task = {
  id: TaskId;
  title: string;
  status: TaskStatus;
  createdAt: string;
  completedAt?: string;
};
```

Do not trust API JSON blindly. Treat responses as `unknown`, validate them, and only then return typed values.

Add tests for successful parsing, invalid response data, not-found responses, and request failures.

## Exercises

1. Define task types.
2. Write typed fetch helpers.
3. Validate unknown API responses.
4. Add a generic `Result<T>` type.
5. Test success and failure paths.
