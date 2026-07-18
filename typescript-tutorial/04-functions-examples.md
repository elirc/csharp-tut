# 04 - Functions Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `function add(a: number, b: number): number` | Typed parameters and return. |
| 2 | `const add = (a: number, b: number) => a + b` | Arrow function with typed parameters. |
| 3 | `(): void` | Function returns no useful value. |
| 4 | `(): Promise<User>` | Async function returns a user promise. |
| 5 | `name?: string` | Optional parameter. |
| 6 | `name = "Guest"` | Default parameter. |
| 7 | `...values: number[]` | Rest parameter. |
| 8 | `(value: string) => boolean` | Function type. |
| 9 | `type Predicate<T> = (value: T) => boolean` | Reusable callback type. |
| 10 | `interface Handler { (event: Event): void }` | Call signature in an interface. |
| 11 | `function overloads` | Multiple call signatures for one implementation. |
| 12 | `this: void` | Declares a function should not use `this`. |
| 13 | `never return` | Function always throws or loops. |
| 14 | `unknown parameter` | Requires narrowing inside function. |
| 15 | `generic function` | Function with caller-chosen type. |
| 16 | `readonly T[]` | Prevents mutation of array parameter. |
| 17 | `as const parameter` | Preserves literal values passed to function. |
| 18 | `ReturnType<typeof fn>` | Gets a function's return type. |
| 19 | `Parameters<typeof fn>` | Gets a function's parameter tuple. |
| 20 | `satisfies function type` | Checks a function against an expected shape. |
