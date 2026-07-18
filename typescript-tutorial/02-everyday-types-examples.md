# 02 - Everyday Types Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `let title: string;` | Declares a string variable. |
| 2 | `let count: number;` | Declares a number variable. |
| 3 | `let ready: boolean;` | Declares a boolean variable. |
| 4 | `const names: string[] = [];` | Array of strings. |
| 5 | `const scores: Array<number> = [];` | Generic array syntax. |
| 6 | `let value: null = null;` | Explicit null type. |
| 7 | `let value: undefined = undefined;` | Explicit undefined type. |
| 8 | `let id: string \| number;` | Union of string or number. |
| 9 | `type Task = { title: string };` | Object type alias. |
| 10 | `interface User { name: string }` | Object interface. |
| 11 | `nickname?: string` | Optional property. |
| 12 | `readonly id: string` | Read-only property. |
| 13 | `Record<string, number>` | Object with string keys and number values. |
| 14 | `unknown` | Must be narrowed before use. |
| 15 | `any` | Avoid unless intentionally opting out. |
| 16 | `void` | Common return type for side-effect functions. |
| 17 | `never` | Represents impossible values. |
| 18 | `"open" \| "closed"` | String literal union. |
| 19 | `const status = "open" as const` | Preserves literal type. |
| 20 | `Date` | Type for JavaScript Date objects. |
