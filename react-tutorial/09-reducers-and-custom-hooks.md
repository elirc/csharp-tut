# 09 - Reducers and Custom Hooks

Use `useReducer` when state transitions become complex or when related state changes should be described as actions.

```jsx
function reducer(state, action) {
  switch (action.type) {
    case "added":
      return [...state, action.task];
    case "removed":
      return state.filter((task) => task.id !== action.id);
    default:
      return state;
  }
}

const [tasks, dispatch] = useReducer(reducer, []);
```

Custom hooks let you reuse stateful logic between components.

```jsx
function useToggle(initial = false) {
  const [value, setValue] = useState(initial);
  const toggle = () => setValue((current) => !current);
  return [value, toggle];
}
```

A custom hook is just a function that calls React hooks and starts with `use`.

## Exercises

1. Convert multiple related `useState` calls to `useReducer`.
2. Add actions for add, update, and remove.
3. Write a `useLocalStorage` hook.
4. Write a `useFetch` hook.
