# 15 - React With TypeScript Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `type Props = { title: string };` | Defines component props. |
| 2 | `function Card({ title }: Props) {}` | Uses typed props. |
| 3 | `children: React.ReactNode` | Types nested JSX content. |
| 4 | `onClick: () => void` | Types a callback prop. |
| 5 | `disabled?: boolean` | Optional boolean prop. |
| 6 | `useState<string>("")` | Types string state. |
| 7 | `useState<User \| null>(null)` | State that starts empty and later holds data. |
| 8 | `React.ChangeEvent<HTMLInputElement>` | Input change event type. |
| 9 | `React.FormEvent<HTMLFormElement>` | Form submit event type. |
| 10 | `useRef<HTMLInputElement \| null>(null)` | Types an input ref. |
| 11 | `Array<Task>` | Array of task objects. |
| 12 | `Task[]` | Short array syntax. |
| 13 | `"idle" \| "loading" \| "error" \| "success"` | Union for request state. |
| 14 | `Record<string, boolean>` | Object with string keys and boolean values. |
| 15 | `Dispatch<Action>` | Common reducer dispatch type. |
| 16 | `Reducer<State, Action>` | Reducer function type. |
| 17 | `as const` | Preserves literal types. |
| 18 | `ComponentProps<"button">` | Reuses native button prop types. |
| 19 | `Omit<Props, "id">` | Removes one prop from a type. |
| 20 | `unknown` | Safer type for untrusted values. |
