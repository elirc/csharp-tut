# 01 - TypeScript Mental Model Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `const name: string = "Ada";` | Explicit string annotation. |
| 2 | `const name = "Ada";` | TypeScript infers `string` or a literal type depending on context. |
| 3 | `let count = 0;` | Inferred as `number`. |
| 4 | `count = "one";` | Compiler rejects assigning string to number. |
| 5 | `type User = { id: string };` | Creates a named object shape. |
| 6 | `const user: User = { id: "1" };` | Value satisfies the required shape. |
| 7 | `type annotations disappear` | Types are erased from emitted JavaScript. |
| 8 | `structural typing` | Compatibility is based on shape, not type name. |
| 9 | `const x = 1 as number;` | Type assertion changes compiler view, not runtime value. |
| 10 | `unknown` | Represents a value you must inspect before using. |
| 11 | `any` | Turns off type checking for a value. |
| 12 | `never` | Represents a value that should never occur. |
| 13 | `void` | Function return type for no useful returned value. |
| 14 | `type UserId = string;` | Alias gives a meaningful name to a type. |
| 15 | `interface User { id: string }` | Interface also describes object shape. |
| 16 | `satisfies` | Checks a value against a type while preserving useful inference. |
| 17 | `as const` | Preserves literal and readonly inference. |
| 18 | `compile-time only` | TypeScript prevents many mistakes before running. |
| 19 | `runtime validation still needed` | External data can still be invalid at runtime. |
| 20 | `editor autocomplete` | Types power better tooling and navigation. |
