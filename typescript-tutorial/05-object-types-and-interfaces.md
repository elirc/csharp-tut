# 05 - Object Types and Interfaces

Object types describe the shape of objects.

```ts
type User = {
  id: string;
  name: string;
  email?: string;
};
```

Interfaces also describe object shapes:

```ts
interface User {
  id: string;
  name: string;
}
```

Both are common. Use either consistently with your team. Type aliases can represent unions and more complex type expressions. Interfaces can merge declarations and are often used for object contracts.

Readonly properties prevent reassignment:

```ts
type Task = {
  readonly id: string;
  title: string;
};
```

Index signatures and `Record` model objects with dynamic keys:

```ts
type Counts = Record<string, number>;
```

## Exercises

1. Create a type alias for a task.
2. Create an interface for a user.
3. Add optional and readonly properties.
4. Model a dictionary of counts.
