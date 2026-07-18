# 11 - Styling and Component Design

React does not require one styling approach. Teams use plain CSS, CSS modules, utility classes, CSS-in-JS, design systems, or component libraries.

Start with component boundaries. A good component has a clear job, obvious props, and limited knowledge of the rest of the app.

```jsx
function Alert({ tone = "info", children }) {
  return <div className={`alert alert-${tone}`}>{children}</div>;
}
```

Prefer composition over giant configurable components. If a component needs many unrelated props, it may be doing too much.

Styling should support accessibility and responsive behavior. A button should look like a button, be keyboard accessible, expose useful text, and show disabled/loading states clearly.

## Exercises

1. Create a reusable `Button`.
2. Create an `Alert` with tone variants.
3. Split a large component into smaller pieces.
4. Add loading, disabled, and empty states.
