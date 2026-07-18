# 02 - Types and Values

JavaScript has primitive values and objects. Common primitives are `string`, `number`, `boolean`, `undefined`, `null`, `bigint`, and `symbol`.

```js
const title = "JavaScript";
const price = 19.99;
const active = true;
const missing = null;
let notAssigned;
```

`undefined` usually means a value has not been assigned. `null` usually means "intentionally empty." Teams vary, but you should be consistent.

JavaScript numbers are floating-point values. That means decimals can surprise you:

```js
console.log(0.1 + 0.2); // 0.30000000000000004
```

Objects and arrays are reference values:

```js
const a = { count: 1 };
const b = a;
b.count = 2;
console.log(a.count); // 2
```

Equality has two common operators. Use `===` and `!==` by default because they avoid implicit type conversion.

```js
console.log(1 === "1"); // false
console.log(1 == "1");  // true
```

## Exercises

1. Compare `null` and `undefined`.
2. Try `==` and `===` with numbers and strings.
3. Copy an object reference and mutate it.
4. Use `Number.isNaN` to check invalid numeric conversion.
