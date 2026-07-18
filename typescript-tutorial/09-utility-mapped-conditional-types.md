# 09 - Utility, Mapped, and Conditional Types

TypeScript can transform types.

Utility types are built in:

```ts
type PartialUser = Partial<User>;
type UserWithoutPassword = Omit<User, "password">;
```

Mapped types transform each property in an object type:

```ts
type Flags<T> = {
  [K in keyof T]: boolean;
};
```

Conditional types choose a type based on another type:

```ts
type IsString<T> = T extends string ? true : false;
```

Use type manipulation to remove duplication and keep types connected. Do not build complex types just to be clever. If a simple type is clearer, use the simple type.

## Exercises

1. Use `Partial`, `Pick`, and `Omit`.
2. Create a mapped type that makes every property nullable.
3. Create a conditional type.
4. Refactor duplicated types into derived types.
