# 06 - Arrays and Collections

Arrays are ordered collections.

```js
const names = ["Ada", "Grace", "Linus"];
console.log(names[0]);
```

Use array methods instead of manual loops when the operation is a common transformation.

```js
const activeUsers = users.filter((user) => user.active);
const names = users.map((user) => user.name);
```

`Map` stores key/value pairs where keys can be any value. `Set` stores unique values.

```js
const counts = new Map();
counts.set("error", 1);

const tags = new Set(["js", "web", "js"]);
```

Know mutation versus copying. Methods like `push` mutate. Methods like `map` return a new array.

## Exercises

1. Filter numbers greater than 10.
2. Map users to email addresses.
3. Count words with a `Map`.
4. Remove duplicates with a `Set`.
