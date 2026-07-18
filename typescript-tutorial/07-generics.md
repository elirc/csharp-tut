# 07 - Generics

Generics let you write reusable code while preserving type information.

```ts
function identity<T>(value: T): T {
  return value;
}
```

The caller chooses `T`, often through inference:

```ts
const name = identity("Ada"); // string
const count = identity(3);    // number
```

Generic constraints limit what types are allowed:

```ts
function getId<T extends { id: string }>(value: T) {
  return value.id;
}
```

Generics are powerful, but do not add them just to look advanced. If a function only works on strings, use `string`. Use generics when the type genuinely varies and should be preserved.

## Exercises

1. Write a generic identity function.
2. Write a generic `first` function.
3. Add a constraint requiring an `id`.
4. Create a generic `Result<T>` type.
