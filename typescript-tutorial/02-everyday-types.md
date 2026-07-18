# 02 - Everyday Types

Everyday TypeScript starts with the types you already know from JavaScript: strings, numbers, booleans, arrays, objects, nullish values, and functions.

```ts
const title: string = "Task";
const count: number = 3;
const done: boolean = false;
```

Arrays can be written two ways:

```ts
const names: string[] = ["Ada", "Grace"];
const scores: Array<number> = [90, 100];
```

Objects can be typed inline or with a named alias:

```ts
type Task = {
  id: string;
  title: string;
  completed: boolean;
};
```

Optional properties use `?`:

```ts
type User = {
  id: string;
  nickname?: string;
};
```

Use `unknown` for values you do not trust yet. Avoid `any` unless you are deliberately escaping the type system.

## Exercises

1. Type a string, number, and boolean.
2. Type an array of tasks.
3. Create a type alias for a user.
4. Add an optional property.
5. Replace an `any` with `unknown` and narrow it.
