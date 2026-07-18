# 02 - Components and Props Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `function Header() { return <h1 />; }` | Defines a function component. |
| 2 | `<Header />` | Renders a component. |
| 3 | `function UserCard({ user }) {}` | Receives an object prop. |
| 4 | `<UserCard user={user} />` | Passes an object prop. |
| 5 | `function Button({ children }) {}` | Receives nested JSX content. |
| 6 | `<Button>Save</Button>` | Passes children to a component. |
| 7 | `function Badge({ tone = "neutral" }) {}` | Gives a prop a default value. |
| 8 | `<Badge tone="success" />` | Passes a string prop. |
| 9 | `<Button disabled={true} />` | Passes a boolean prop. |
| 10 | `<List items={tasks} />` | Passes an array prop. |
| 11 | `<Form onSubmit={saveTask} />` | Passes a callback prop. |
| 12 | `props.title` | Reads a prop without destructuring. |
| 13 | `{ title, subtitle }` | Destructures props. |
| 14 | `<Avatar size={48} />` | Passes a number prop. |
| 15 | `<Icon ariaLabel="Search" />` | Passes accessibility text. |
| 16 | `<Card className="featured" />` | Allows parent styling hooks. |
| 17 | `<Modal open={isOpen} />` | Controls display with a prop. |
| 18 | `<TodoItem key={todo.id} todo={todo} />` | Passes item data while rendering lists. |
| 19 | `onClick={() => onSelect(id)}` | Wraps a callback to pass an argument. |
| 20 | `export default ProfileCard;` | Exports a component from a file. |
