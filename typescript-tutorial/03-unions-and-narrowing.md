# 03 - Unions and Narrowing

A union type means a value can be one of several types.

```ts
function formatId(id: string | number) {
  return String(id);
}
```

Before using members specific to one side of a union, narrow the type.

```ts
function pad(value: string | number) {
  if (typeof value === "number") {
    return value.toFixed(2);
  }

  return value.trim();
}
```

TypeScript uses JavaScript control flow to narrow types: `typeof`, `instanceof`, truthiness checks, equality checks, `in`, discriminated unions, and custom type guards.

Discriminated unions are one of TypeScript's most useful modeling tools:

```ts
type Result =
  | { ok: true; value: string }
  | { ok: false; error: string };
```

Checking `result.ok` tells TypeScript which shape you have.

## Exercises

1. Create a `string | number` union.
2. Narrow with `typeof`.
3. Create a success/failure discriminated union.
4. Add an exhaustive switch with `never`.
