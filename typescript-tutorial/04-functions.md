# 04 - Functions

Function types describe parameters and return values.

```ts
function add(a: number, b: number): number {
  return a + b;
}
```

Arrow functions can be typed too:

```ts
const formatName = (name: string): string => name.trim();
```

Callbacks are common in JavaScript and TypeScript:

```ts
function filter<T>(items: T[], predicate: (item: T) => boolean): T[] {
  return items.filter(predicate);
}
```

Optional parameters use `?`, default parameters infer from their default, and rest parameters describe variable-length inputs.

Avoid over-typing simple local functions when TypeScript can infer clearly. Add return types at module boundaries, public APIs, and places where a wrong inferred return would cause confusion.

## Exercises

1. Type a function with parameters and return value.
2. Type a callback parameter.
3. Add an optional parameter.
4. Write a function overload.
