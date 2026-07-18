# 04 - Functions, Scope, and Closures Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `function greet(name) { return \`Hi ${name}\`; }` | Declares a named function. |
| 2 | `const greet = (name) => \`Hi ${name}\`;` | Declares an arrow function. |
| 3 | `const add = (a, b) => a + b;` | Returns an expression directly. |
| 4 | `function log(message = "Hi") {}` | Uses a default parameter. |
| 5 | `function sum(...numbers) {}` | Uses rest parameters. |
| 6 | `callback(value);` | Invokes a function passed as a parameter. |
| 7 | `items.map((x) => x.id)` | Passes an arrow function as a callback. |
| 8 | `return function inner() {};` | Returns a function from a function. |
| 9 | `let count = 0;` | Creates mutable state in a scope. |
| 10 | `() => ++count` | Captures and updates outer state. |
| 11 | `{ const local = 1; }` | Creates a block-scoped variable. |
| 12 | `function outer() { function inner() {} }` | Nests a helper function. |
| 13 | `const fn = console.log;` | Stores a function in a variable. |
| 14 | `setTimeout(() => run(), 1000)` | Runs a callback later. |
| 15 | `array.filter(Boolean)` | Uses a function to remove falsy values. |
| 16 | `function* ids() { yield 1; }` | Declares a generator function. |
| 17 | `async function load() {}` | Declares an async function. |
| 18 | `const load = async () => {};` | Declares an async arrow function. |
| 19 | `this` | Depends on how a function is called. |
| 20 | `fn.bind(object)` | Creates a function with fixed `this`. |
