# 15 - Testing and Refactoring With Types

TypeScript is not a replacement for tests. Types catch many mistakes, but they do not prove behavior is correct.

Types answer questions like:

- Is this property available?
- Did I handle null?
- Can this function receive this value?
- Did this refactor break call sites?

Tests answer questions like:

- Does this function return the right result?
- Does this UI behave correctly?
- Does this API handle invalid input?
- Does this bug stay fixed?

TypeScript helps refactoring because the compiler points to broken assumptions. Rename a field, change a function return type, or add a new union variant, then let errors guide you through required changes.

## Exercises

1. Write a typed function and unit test it.
2. Change a type and follow compiler errors.
3. Add a new union variant and make the switch exhaustive.
4. Use `@ts-expect-error` for an intentional type-level test.
