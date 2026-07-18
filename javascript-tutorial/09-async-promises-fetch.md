# 09 - Async, Promises, and Fetch

JavaScript is heavily asynchronous. A `Promise` represents work that may finish later.

```js
const promise = fetch("https://example.com");
```

Use `async` and `await` to write promise-based code in a readable style:

```js
async function loadUser(id) {
  const response = await fetch(`/api/users/${id}`);
  if (!response.ok) {
    throw new Error(`HTTP ${response.status}`);
  }
  return await response.json();
}
```

`await` pauses the current async function, not the entire JavaScript runtime. Multiple independent operations can run concurrently with `Promise.all`.

```js
const [user, orders] = await Promise.all([
  loadUser(id),
  loadOrders(id)
]);
```

## Exercises

1. Write an async function that waits with `setTimeout`.
2. Fetch JSON from an API.
3. Handle a non-200 response.
4. Run two async operations with `Promise.all`.
