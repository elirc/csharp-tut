# 03 - Control Flow Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `if (ready) start();` | Runs code only when a condition is truthy. |
| 2 | `if (!ready) return;` | Uses an early return for a guard. |
| 3 | `else if (score >= 80)` | Adds another branch. |
| 4 | `const label = count === 1 ? "item" : "items";` | Chooses between two values. |
| 5 | `switch (command) {}` | Branches on one value. |
| 6 | `case "quit": break;` | Handles one switch case. |
| 7 | `default:` | Handles unmatched switch cases. |
| 8 | `for (let i = 0; i < 5; i++) {}` | Loops with an index. |
| 9 | `for (const item of items) {}` | Loops through iterable values. |
| 10 | `for (const key in object) {}` | Loops through enumerable object keys. |
| 11 | `while (queue.length > 0) {}` | Repeats while a condition is true. |
| 12 | `do { read(); } while (!done);` | Runs at least once. |
| 13 | `break;` | Exits a loop or switch. |
| 14 | `continue;` | Skips to the next loop iteration. |
| 15 | `return value;` | Exits a function with a value. |
| 16 | `items.length === 0` | Checks for an empty array. |
| 17 | `name?.length > 0` | Safely reads a maybe-missing property. |
| 18 | `value >= min && value <= max` | Checks an inclusive range. |
| 19 | `role === "admin" \|\| role === "owner"` | Checks alternatives. |
| 20 | `throw new Error("Invalid state");` | Stops normal control flow with an error. |
