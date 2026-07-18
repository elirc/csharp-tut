# 03 - Unions and Narrowing Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `string \| number` | Value may be string or number. |
| 2 | `typeof value === "string"` | Narrows to string. |
| 3 | `typeof value === "number"` | Narrows to number. |
| 4 | `value instanceof Date` | Narrows to Date. |
| 5 | `"id" in value` | Checks property existence. |
| 6 | `if (value)` | Narrows away some falsy values. |
| 7 | `value === null` | Narrows to null. |
| 8 | `value != null` | Narrows away null and undefined. |
| 9 | `Array.isArray(value)` | Narrows to array. |
| 10 | `type Result = Success \| Failure` | Models alternatives. |
| 11 | `{ kind: "loading" }` | Discriminant property. |
| 12 | `switch (state.kind)` | Narrows discriminated union cases. |
| 13 | `case "success":` | Handles one union variant. |
| 14 | `const _exhaustive: never = state` | Exhaustiveness check. |
| 15 | `function isUser(value): value is User` | Custom type guard. |
| 16 | `asserts value is User` | Assertion function type. |
| 17 | `unknown to specific` | Narrow unknown before using it. |
| 18 | `optional chaining` | Handles maybe-null values safely. |
| 19 | `nullish coalescing` | Provides fallback for null or undefined. |
| 20 | `control flow analysis` | TypeScript tracks narrowing through branches. |
