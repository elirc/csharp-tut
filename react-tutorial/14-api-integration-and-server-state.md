# 14 - API Integration and Server State

React apps often talk to APIs. Data from a server is different from local UI state. It has loading, error, caching, refetching, stale data, and synchronization concerns.

For small apps, an effect with `fetch` can be fine:

```jsx
useEffect(() => {
  let ignore = false;

  async function loadTasks() {
    setLoading(true);
    try {
      const response = await fetch("/api/tasks");
      const data = await response.json();
      if (!ignore) setTasks(data);
    } catch (error) {
      if (!ignore) setError(error.message);
    } finally {
      if (!ignore) setLoading(false);
    }
  }

  loadTasks();
  return () => {
    ignore = true;
  };
}, []);
```

For larger apps, use a server-state library such as TanStack Query or a framework data layer. These tools handle caching, retries, refetching, and mutation states.

## Exercises

1. Fetch tasks and render loading, error, empty, and success states.
2. Add a create-task mutation.
3. Refetch after saving.
4. Identify what is local UI state versus server state.
