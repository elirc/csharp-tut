# 04 - Conditional Rendering and Lists

React UI is a function of state and props. Conditional rendering means showing different UI for different data.

```jsx
if (isLoading) {
  return <Spinner />;
}

if (error) {
  return <ErrorMessage message={error} />;
}

return <TaskList tasks={tasks} />;
```

Lists are rendered with `map`. Every rendered list item needs a stable `key`.

```jsx
function TaskList({ tasks }) {
  return (
    <ul>
      {tasks.map((task) => (
        <li key={task.id}>{task.title}</li>
      ))}
    </ul>
  );
}
```

Do not use array indexes as keys if items can be inserted, removed, filtered, or reordered.

## Exercises

1. Render loading, error, empty, and success states.
2. Render a list of tasks.
3. Add stable keys.
4. Filter completed tasks before rendering.
