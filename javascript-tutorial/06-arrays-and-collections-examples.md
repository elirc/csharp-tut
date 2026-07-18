# 06 - Arrays and Collections Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `const items = [];` | Creates an empty array. |
| 2 | `items.push("a")` | Adds to the end of an array. |
| 3 | `items.pop()` | Removes from the end. |
| 4 | `items.unshift("a")` | Adds to the beginning. |
| 5 | `items.shift()` | Removes from the beginning. |
| 6 | `items.length` | Reads array size. |
| 7 | `items.includes("a")` | Checks for a value. |
| 8 | `items.indexOf("a")` | Finds the first index. |
| 9 | `items.find((x) => x.id === id)` | Finds the first matching item. |
| 10 | `items.some((x) => x.done)` | Checks whether any item matches. |
| 11 | `items.every((x) => x.done)` | Checks whether all items match. |
| 12 | `items.filter((x) => x.active)` | Keeps matching items. |
| 13 | `items.map((x) => x.name)` | Transforms each item. |
| 14 | `items.reduce((sum, x) => sum + x, 0)` | Reduces to one value. |
| 15 | `items.sort((a, b) => a - b)` | Sorts numbers in place. |
| 16 | `new Set(items)` | Creates a unique-value collection. |
| 17 | `[...new Set(items)]` | Removes duplicates from an array. |
| 18 | `new Map()` | Creates a key/value collection. |
| 19 | `map.set(key, value)` | Adds or updates a map entry. |
| 20 | `map.get(key)` | Reads a map entry. |
