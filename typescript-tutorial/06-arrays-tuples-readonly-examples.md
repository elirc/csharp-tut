# 06 - Arrays, Tuples, and Readonly Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `string[]` | Array of strings. |
| 2 | `Array<number>` | Generic array syntax. |
| 3 | `User[]` | Array of user objects. |
| 4 | `readonly string[]` | Read-only string array. |
| 5 | `ReadonlyArray<number>` | Generic readonly array syntax. |
| 6 | `[number, number]` | Tuple with two numbers. |
| 7 | `[id: string, count: number]` | Named tuple elements. |
| 8 | `[string, ...number[]]` | Tuple with rest elements. |
| 9 | `as const` | Infers readonly literal tuple/object values. |
| 10 | `const pair = [1, 2] as const` | Readonly tuple of literal values. |
| 11 | `items.map(...)` | Keeps array element type through mapping. |
| 12 | `items.filter(Boolean)` | Runtime filter with imperfect type narrowing. |
| 13 | `items.find(...)` | Returns item or undefined. |
| 14 | `items.reduce<number>(...)` | Explicit reduce accumulator type. |
| 15 | `Readonly<T>` | Makes object properties readonly. |
| 16 | `readonly id: string` | Read-only object property. |
| 17 | `push not allowed` | Readonly arrays cannot be pushed through that reference. |
| 18 | `[...items, item]` | Immutable array add. |
| 19 | `items.filter(x => x.id !== id)` | Immutable array remove. |
| 20 | `items.map(x => x.id === id ? next : x)` | Immutable array update. |
