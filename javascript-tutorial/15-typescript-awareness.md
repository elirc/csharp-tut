# 15 - TypeScript Awareness

TypeScript is JavaScript with static type checking. Many professional JavaScript teams use it because it catches mistakes earlier and documents intent.

```ts
type User = {
  id: string;
  name: string;
};

function greet(user: User): string {
  return `Hi, ${user.name}`;
}
```

You can learn JavaScript first and still benefit from TypeScript later. The key is to understand that TypeScript does not replace runtime validation. It checks your code before it runs, but API responses, user input, files, and databases can still contain invalid data.

Learn enough TypeScript to read typed code: interfaces, type aliases, function types, unions, generics, optional properties, and narrowing.

## Exercises

1. Convert a JavaScript function to TypeScript.
2. Add a type for a user object.
3. Use a union type for status values.
4. Narrow an unknown value before using it.
