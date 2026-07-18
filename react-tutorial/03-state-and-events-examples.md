# 03 - State and Events Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `const [count, setCount] = useState(0);` | Declares numeric state. |
| 2 | `setCount(count + 1)` | Sets state from current render value. |
| 3 | `setCount(c => c + 1)` | Safely updates from previous state. |
| 4 | `const [open, setOpen] = useState(false);` | Declares boolean state. |
| 5 | `setOpen(o => !o)` | Toggles boolean state. |
| 6 | `onClick={handleClick}` | Subscribes to click events. |
| 7 | `onChange={handleChange}` | Handles input changes. |
| 8 | `onSubmit={handleSubmit}` | Handles form submission. |
| 9 | `event.preventDefault()` | Stops default form behavior. |
| 10 | `event.target.value` | Reads input text from an event. |
| 11 | `setItems([...items, item])` | Adds to array state immutably. |
| 12 | `setItems(items.filter(x => x.id !== id))` | Removes from array state immutably. |
| 13 | `setUser({ ...user, name })` | Updates object state immutably. |
| 14 | `useState(() => loadInitial())` | Lazily computes initial state. |
| 15 | `disabled={isSaving}` | Uses state to control an element. |
| 16 | `{error && <p>{error}</p>}` | Renders state-driven feedback. |
| 17 | `setError(null)` | Clears state. |
| 18 | `setSelectedId(id)` | Stores selected item identity. |
| 19 | `onKeyDown={handleKeyDown}` | Handles keyboard events. |
| 20 | `onBlur={validate}` | Runs logic when an input loses focus. |
