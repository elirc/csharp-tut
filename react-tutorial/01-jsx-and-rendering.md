# 01 - JSX and Rendering

JSX is JavaScript syntax that describes UI. It looks like HTML, but it is JavaScript expressions under the hood.

```jsx
function App() {
  return <h1>Hello, React</h1>;
}
```

You can embed JavaScript expressions with braces:

```jsx
const name = "Ada";
return <h1>Hello, {name}</h1>;
```

JSX has a few important rules. Return one parent element, use `className` instead of `class`, use `htmlFor` instead of `for`, and close every tag.

```jsx
return (
  <section className="card">
    <h2>{title}</h2>
    <p>{description}</p>
  </section>
);
```

React renders components into the DOM and updates the DOM when data changes.

## Exercises

1. Render a heading and paragraph.
2. Insert a variable into JSX.
3. Add a CSS class with `className`.
4. Return multiple elements using a fragment.
