# 04 - Conditional Rendering and Lists Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `if (loading) return <Spinner />;` | Early return for loading UI. |
| 2 | `if (error) return <Error message={error} />;` | Early return for error UI. |
| 3 | `{isOpen && <Modal />}` | Renders only when truthy. |
| 4 | `{count > 0 ? <List /> : <Empty />}` | Chooses between two UI branches. |
| 5 | `return null;` | Renders nothing. |
| 6 | `tasks.map(task => <li key={task.id}>{task.title}</li>)` | Renders list items with stable keys. |
| 7 | `key={task.id}` | Helps React match list items between renders. |
| 8 | `tasks.filter(t => !t.done).map(...)` | Filters before rendering. |
| 9 | `tasks.length === 0` | Detects empty data. |
| 10 | `<>{items}</>` | Wraps sibling list output in a fragment. |
| 11 | `users.map(user => <UserRow user={user} />)` | Renders a component per item. |
| 12 | `key={user.email}` | Uses a stable unique field as a key. |
| 13 | `{message ?? "No message"}` | Renders a fallback for nullish values. |
| 14 | `{user?.name}` | Safely renders optional nested data. |
| 15 | `{Boolean(count) && <Badge />}` | Forces a numeric condition to boolean. |
| 16 | `{items.slice(0, 5).map(...)}` | Renders a limited list. |
| 17 | `{group.items.map(...)}` | Renders nested data. |
| 18 | `<Fragment key={id}>...</Fragment>` | Uses a keyed fragment. |
| 19 | `array index as key` | Only safe for static lists that never reorder. |
| 20 | `sort before map` | Produces predictable list order. |
