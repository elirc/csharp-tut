# 01 - JSX and Rendering Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `<h1>Hello</h1>` | JSX element describing a heading. |
| 2 | `<App />` | Renders a component. |
| 3 | `{name}` | Inserts a JavaScript expression into JSX. |
| 4 | `{count + 1}` | Expressions can calculate values. |
| 5 | `className="card"` | Adds a CSS class in JSX. |
| 6 | `htmlFor="email"` | Connects a label to an input. |
| 7 | `<img alt="Profile" />` | Self-closes a tag. |
| 8 | `<>...</>` | Fragment for returning siblings. |
| 9 | `{isReady && <p>Ready</p>}` | Conditionally renders a node. |
| 10 | `{items.map(item => <li>{item}</li>)}` | Renders a list from data. |
| 11 | `style={{ color: "red" }}` | Adds inline styles as an object. |
| 12 | `aria-label="Close"` | Adds accessible label text. |
| 13 | `<button disabled={isSaving}>Save</button>` | Binds a boolean prop. |
| 14 | `<input value={name} />` | Binds a value prop. |
| 15 | `{user?.name}` | Safely renders optional data. |
| 16 | `{total.toFixed(2)}` | Formats data before rendering. |
| 17 | `return null;` | Renders nothing from a component. |
| 18 | `<section>{children}</section>` | Renders nested content. |
| 19 | `{/* comment */}` | Writes a JSX comment. |
| 20 | `<React.StrictMode>` | Development wrapper for extra checks. |
