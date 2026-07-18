# 13 - TypeScript with DOM, Node, and React

TypeScript changes depending on the environment. Browser projects use DOM types. Node projects use Node types. React projects use JSX and React component types.

DOM example:

```ts
const input = document.querySelector<HTMLInputElement>("#email");
console.log(input?.value);
```

Node example:

```ts
import { readFile } from "node:fs/promises";

const text = await readFile("notes.txt", "utf8");
```

React example:

```tsx
type ButtonProps = {
  children: React.ReactNode;
  onClick: () => void;
};
```

The key is knowing which type libraries are available. Browser types come from the `DOM` lib. Node types usually come from `@types/node`.

## Exercises

1. Type a DOM input query.
2. Install and use Node types.
3. Type React component props.
4. Type a form submit event.
