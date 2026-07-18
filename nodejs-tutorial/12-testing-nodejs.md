# 12 - Testing Node.js

Node includes a built-in test runner through `node:test`.

```js
import test from "node:test";
import assert from "node:assert/strict";

test("adds numbers", () => {
  assert.equal(add(2, 3), 5);
});
```

Run tests:

```bash
node --test
```

You can also use ecosystem tools such as Vitest, Jest, Mocha, Supertest, MSW, or Playwright depending on the app.

Test pure logic first. Then test HTTP routes, file behavior, database boundaries, and failure cases. Good tests are part of backend readiness.

## Exercises

1. Write a built-in `node:test` test.
2. Assert equality with `node:assert/strict`.
3. Test an async function.
4. Test an HTTP endpoint.
5. Add a failing test before fixing a bug.
