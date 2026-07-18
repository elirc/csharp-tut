# 01 - TypeScript Mental Model

TypeScript checks code statically. It does not change JavaScript runtime behavior. After TypeScript compiles, your code runs as JavaScript.

```ts
const count: number = 3;
```

The type annotation helps the compiler, editor, and reader. It does not exist at runtime.

TypeScript can infer many types:

```ts
const name = "Ada"; // string
const count = 3;    // number
```

Use annotations when they clarify intent, define boundaries, or prevent inference from being too wide or too narrow. You do not need to annotate everything.

TypeScript is structural. If an object has the required shape, it can be used as that type.

```ts
type User = { id: string; name: string };

const value = { id: "1", name: "Ada", role: "admin" };
const user: User = value;
```

The extra `role` property is allowed because the value has at least the required shape.

## Exercises

1. Write a value with an explicit type.
2. Remove the type and observe inference.
3. Create a structural type and assign a compatible object.
4. Try to use a type annotation at runtime and see why it does not exist.
