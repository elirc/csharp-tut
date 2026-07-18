# 04 - Functions, Scope, and Closures

Functions package behavior. They can accept parameters, return values, and be passed around like any other value.

```js
function add(a, b) {
  return a + b;
}
```

Arrow functions are common for callbacks and short expressions:

```js
const square = (n) => n * n;
```

JavaScript has block scope for `let` and `const`. A variable declared inside a block is not available outside it.

Closures happen when a function remembers variables from the scope where it was created.

```js
function createCounter() {
  let count = 0;
  return () => {
    count += 1;
    return count;
  };
}
```

Closures are powerful. They enable callbacks, factories, private state, event handlers, and many async patterns.

## Exercises

1. Write a function declaration and an arrow function.
2. Create a function that returns another function.
3. Build a counter with private state.
4. Pass a function into another function as a callback.
