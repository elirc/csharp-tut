# 10 - Classes and Access Modifiers

TypeScript supports JavaScript classes plus type annotations and access modifiers.

```ts
class User {
  constructor(public readonly id: string, public name: string) {}

  rename(name: string) {
    this.name = name;
  }
}
```

Access modifiers describe how class members should be used:

- `public`: available everywhere.
- `private`: available only inside the class.
- `protected`: available inside the class and subclasses.
- `readonly`: can only be assigned during initialization.

TypeScript `private` is a compile-time restriction. JavaScript `#private` fields are runtime-private.

Classes can implement interfaces:

```ts
interface Logger {
  log(message: string): void;
}

class ConsoleLogger implements Logger {
  log(message: string) {
    console.log(message);
  }
}
```

## Exercises

1. Create a class with constructor parameter properties.
2. Add a private field.
3. Implement an interface.
4. Compare `private` and `#private`.
