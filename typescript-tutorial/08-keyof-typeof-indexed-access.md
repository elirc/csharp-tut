# 08 - Keyof, Typeof, and Indexed Access

TypeScript can create types from existing values and types.

`keyof` gets the keys of an object type:

```ts
type User = { id: string; name: string };
type UserKey = keyof User; // "id" | "name"
```

`typeof` gets the type of a value:

```ts
const config = { retries: 3, mode: "strict" };
type Config = typeof config;
```

Indexed access gets a property type:

```ts
type UserName = User["name"]; // string
```

These tools are useful when you want types to stay connected instead of duplicating information manually.

## Exercises

1. Use `keyof` on an object type.
2. Use `typeof` on a config object.
3. Use indexed access to get a property type.
4. Write a type-safe `getProperty` helper.
