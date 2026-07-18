# 02 - Types and Values Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `typeof "hi"` | Returns `"string"`. |
| 2 | `typeof 42` | Returns `"number"`. |
| 3 | `typeof true` | Returns `"boolean"`. |
| 4 | `typeof undefined` | Returns `"undefined"`. |
| 5 | `typeof null` | Returns `"object"`, a famous historical quirk. |
| 6 | `const n = 10n;` | Creates a `bigint`. |
| 7 | `Symbol("id")` | Creates a unique symbol value. |
| 8 | `Number("abc")` | Produces `NaN`. |
| 9 | `Number.isNaN(value)` | Safely checks for `NaN`. |
| 10 | `parseInt("42px", 10)` | Parses leading integer text. |
| 11 | `parseFloat("3.14")` | Parses decimal text. |
| 12 | `1 === "1"` | Strict equality returns false. |
| 13 | `1 == "1"` | Loose equality converts types and returns true. |
| 14 | `value ?? "fallback"` | Uses a fallback only for `null` or `undefined`. |
| 15 | `value \|\| "fallback"` | Uses a fallback for any falsy value. |
| 16 | `Boolean("")` | Empty string is falsy. |
| 17 | `Boolean([])` | Arrays are truthy even when empty. |
| 18 | `Array.isArray(value)` | Checks whether a value is an array. |
| 19 | `{ ...user }` | Shallow-copies an object. |
| 20 | `[...items]` | Shallow-copies an array. |
