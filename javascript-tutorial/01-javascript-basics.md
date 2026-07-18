# 01 - JavaScript Basics

JavaScript programs are made of statements and expressions. A statement does something. An expression produces a value.

```js
const name = "Ada";
console.log(`Hello, ${name}`);
```

Use `const` by default. Use `let` when a variable must be reassigned. Avoid `var` in modern code because its function-scoped behavior causes surprises.

```js
const fixed = 10;
let count = 0;
count += 1;
```

JavaScript uses braces for blocks, semicolons are optional but common in many teams, and comments work like C-style languages.

```js
// Single-line comment
/*
  Multi-line comment
*/
```

Template literals use backticks and support interpolation:

```js
const total = 19.99;
console.log(`Total: $${total}`);
```

Most beginner JavaScript code runs in either a browser console or Node. The browser gives you `document`, `window`, and DOM APIs. Node gives you `process`, file APIs, and server-side libraries.

## Exercises

1. Create variables with `const` and `let`.
2. Print interpolated strings.
3. Write a small script that reads two numbers from constants and logs their sum.
4. Rewrite one `let` variable as `const` and see whether the code still works.
