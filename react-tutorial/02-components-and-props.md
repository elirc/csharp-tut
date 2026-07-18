# 02 - Components and Props

Components are reusable UI functions. They receive data through props and return JSX.

```jsx
function Greeting({ name }) {
  return <h1>Hello, {name}</h1>;
}
```

Use components to split UI into meaningful pieces. A component should have a clear reason to exist: a button, a card, a list, a form, a page section, or a feature.

Props flow from parent to child:

```jsx
function App() {
  return <Greeting name="Ada" />;
}
```

Props should be treated as read-only. If a child needs to change something, the parent usually passes a callback prop.

```jsx
function SaveButton({ onSave }) {
  return <button onClick={onSave}>Save</button>;
}
```

## Exercises

1. Create a `ProfileCard` component.
2. Pass `name` and `role` props.
3. Add a callback prop for a button click.
4. Render the same component with different props.
