# 15 - TypeScript Awareness Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `const name: string = "Ada";` | Adds a string annotation. |
| 2 | `let count: number = 0;` | Adds a number annotation. |
| 3 | `type User = { id: string; name: string };` | Defines an object shape. |
| 4 | `interface User { id: string }` | Defines an object contract. |
| 5 | `name?: string` | Marks a property optional. |
| 6 | `string \| null` | Allows either a string or null. |
| 7 | `"open" \| "closed"` | Creates a string-literal union. |
| 8 | `function add(a: number, b: number): number` | Types parameters and return value. |
| 9 | `Array<string>` | Generic array syntax. |
| 10 | `string[]` | Short array syntax. |
| 11 | `Promise<User>` | Async result type. |
| 12 | `Record<string, number>` | Object with string keys and number values. |
| 13 | `Partial<User>` | Makes all properties optional. |
| 14 | `Pick<User, "id">` | Selects part of a type. |
| 15 | `unknown` | Safer alternative to `any` for unknown values. |
| 16 | `any` | Disables type checking for a value. |
| 17 | `if (typeof value === "string")` | Narrows an unknown value. |
| 18 | `value as User` | Type assertion; use carefully. |
| 19 | `function first<T>(items: T[]): T` | Generic function. |
| 20 | `satisfies` | Checks a value against a type without widening as much. |
