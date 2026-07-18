# 14 - Runtime Validation and API Boundaries

TypeScript checks your code before it runs. It does not guarantee that external data is valid. Anything from a user, API, file, database, local storage, or environment variable needs runtime validation.

Bad assumption:

```ts
const user = await response.json() as User;
console.log(user.email.toLowerCase());
```

The assertion tells TypeScript to trust you. It does not validate the response.

Safer approach:

```ts
function isUser(value: unknown): value is User {
  return typeof value === "object" &&
    value !== null &&
    "email" in value &&
    typeof value.email === "string";
}
```

Many teams use validation libraries such as Zod, Valibot, or io-ts so runtime validation and TypeScript types stay connected.

## Exercises

1. Treat API JSON as `unknown`.
2. Write a type guard.
3. Validate an environment variable.
4. Replace unsafe `as User` with runtime validation.
