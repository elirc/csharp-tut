# 03 - Control Flow

Control flow decides which code runs and how many times it runs.

Use `if`, `else if`, and `else` for branching:

```js
if (score >= 90) {
  console.log("A");
} else if (score >= 70) {
  console.log("Pass");
} else {
  console.log("Retry");
}
```

Use `switch` when one value maps to several cases:

```js
switch (role) {
  case "admin":
    console.log("Full access");
    break;
  default:
    console.log("Standard access");
}
```

Loops repeat work. Prefer `for...of` when reading values from an array.

```js
for (const item of items) {
  console.log(item);
}
```

Use `break` to exit a loop and `continue` to skip one iteration.

## Exercises

1. Write a grade calculator.
2. Loop through names and print only names longer than three characters.
3. Rewrite a `for` loop as `for...of`.
4. Use `switch` for a command parser.
