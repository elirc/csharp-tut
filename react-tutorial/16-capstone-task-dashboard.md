# 16 - Capstone: Task Dashboard

Build a small task dashboard. It should include list rendering, forms, state, effects, API calls, routing, tests, and clear component boundaries.

Suggested screens:

```text
/                 dashboard summary
/tasks            task list with filters
/tasks/new        create task form
/tasks/:id        task details
/settings         simple settings page
```

Suggested component structure:

- `App`
- `Layout`
- `TaskListPage`
- `TaskDetailPage`
- `TaskForm`
- `TaskList`
- `TaskItem`
- `StatusFilter`
- `useTasks`
- `api/tasks`

Start with local state. Then replace it with API-backed data. Add loading, error, empty, and success states. Write tests for user behavior, not implementation details.

## Exercises

1. Build the task list with local data.
2. Add create, complete, and delete behavior.
3. Add routing.
4. Replace local data with API calls.
5. Add tests for important user flows.
