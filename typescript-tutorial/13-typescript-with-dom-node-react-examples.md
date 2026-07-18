# 13 - TypeScript with DOM, Node, and React Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `HTMLElement` | Base DOM element type. |
| 2 | `HTMLInputElement` | Input element type. |
| 3 | `document.querySelector<HTMLInputElement>()` | Narrows selected DOM element type. |
| 4 | `Event` | Base browser event type. |
| 5 | `MouseEvent` | Browser mouse event type. |
| 6 | `KeyboardEvent` | Browser keyboard event type. |
| 7 | `addEventListener("click", handler)` | DOM event API with typed event. |
| 8 | `@types/node` | Type declarations for Node APIs. |
| 9 | `process.env.NODE_ENV` | Node environment variable access. |
| 10 | `Buffer` | Node binary data type. |
| 11 | `React.ReactNode` | Type for renderable React children. |
| 12 | `React.ComponentProps<"button">` | Native button prop type. |
| 13 | `React.ChangeEvent<HTMLInputElement>` | React input change event. |
| 14 | `React.FormEvent<HTMLFormElement>` | React form submit event. |
| 15 | `useState<User \| null>` | React state with nullable data. |
| 16 | `useRef<HTMLInputElement \| null>` | React ref for input element. |
| 17 | `.tsx` | TypeScript file extension for JSX. |
| 18 | `"jsx": "react-jsx"` | TSConfig setting for React JSX. |
| 19 | `lib DOM` | Provides browser global types. |
| 20 | `environment-specific types` | Match your TS setup to where code runs. |
