# 08 - Sharing State and Context

When two components need the same state, move that state to their closest common parent. This is called lifting state up.

```jsx
function App() {
  const [selectedId, setSelectedId] = useState(null);
  return (
    <>
      <TaskList selectedId={selectedId} onSelect={setSelectedId} />
      <TaskDetails id={selectedId} />
    </>
  );
}
```

Props are the default way to pass data down. Context is for values many components need, such as theme, current user, locale, or app-level settings.

```jsx
const ThemeContext = createContext("light");

function Button() {
  const theme = useContext(ThemeContext);
  return <button className={theme}>Save</button>;
}
```

Do not use context as a dumping ground for all state. Context makes values easy to access but can make data flow harder to understand if overused.

## Exercises

1. Lift selected item state to a parent.
2. Pass state and callbacks down as props.
3. Create a theme context.
4. Decide whether a value belongs in props, state, or context.
