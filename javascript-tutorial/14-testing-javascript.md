# 14 - Testing JavaScript

Tests protect behavior. They let you change code with confidence.

Vitest is a modern test runner with a Jest-like API:

```bash
npm install -D vitest
```

```js
import { describe, expect, it } from "vitest";
import { add } from "./math.js";

describe("add", () => {
  it("adds two numbers", () => {
    expect(add(2, 3)).toBe(5);
  });
});
```

Test pure functions first. They are the easiest and give the fastest feedback. Then test services, route handlers, and integration flows. Do not only test happy paths. Bugs often live in missing data, invalid input, failed network calls, and edge cases.

## Exercises

1. Add Vitest to a project.
2. Test a pure function.
3. Test an async function.
4. Write one failing test before fixing the code.
