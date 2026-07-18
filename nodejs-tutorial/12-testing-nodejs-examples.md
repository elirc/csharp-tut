# 12 - Testing Node.js Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `import test from "node:test"` | Imports built-in test runner. |
| 2 | `import assert from "node:assert/strict"` | Imports strict assertions. |
| 3 | `test("name", () => {})` | Defines a test. |
| 4 | `assert.equal(actual, expected)` | Checks equality. |
| 5 | `assert.deepEqual(actual, expected)` | Checks object/array equality. |
| 6 | `assert.throws(fn)` | Checks sync error. |
| 7 | `await assert.rejects(promise)` | Checks async rejection. |
| 8 | `node --test` | Runs tests. |
| 9 | `node --test --watch` | Watches tests in supported versions. |
| 10 | `describe` | Groups tests in supported runner style. |
| 11 | `beforeEach` | Per-test setup. |
| 12 | `afterEach` | Per-test cleanup. |
| 13 | `mock.fn()` | Built-in mocking support area. |
| 14 | `supertest` | Common Express integration test helper. |
| 15 | `undici MockAgent` | Mock HTTP calls. |
| 16 | `temporary directory` | Isolated file test workspace. |
| 17 | `fake dependency` | Simple test double. |
| 18 | `integration test` | Tests multiple pieces together. |
| 19 | `unit test` | Tests one small unit. |
| 20 | `CI test command` | Tests should run automatically. |
| 21 | `test.todo("case")` | Records a planned test. |
| 22 | `test.skip("case", fn)` | Temporarily skips a test. |
| 23 | `test.only("case", fn)` | Runs only one test during debugging. |
| 24 | `assert.match(text, /pattern/)` | Checks text with a regular expression. |
| 25 | `assert.doesNotThrow(fn)` | Verifies a function does not throw. |
| 26 | `mock.method(object, "name")` | Mocks a method with built-in mocking APIs. |
| 27 | `t.after(fn)` | Registers cleanup for a specific test context. |
| 28 | `test fixtures` | Known input data used by tests. |
| 29 | `test isolation` | Tests should not depend on each other. |
| 30 | `deterministic tests` | Tests should produce same result every run. |

## Interview Questions

**Q1. What is the difference between a unit test and an integration test?**  
A unit test checks a small piece in isolation. An integration test checks multiple pieces working together.

**Q2. Why should tests be deterministic?**  
They need to pass or fail for real reasons, not timing, order, external state, or random data.

**Q3. How do you test async code in Node?**  
Return or await the promise in the test, then assert the result or rejection.

**Q4. Why test failure cases?**  
Production bugs often happen in invalid input, missing data, rejected promises, and dependency failures.

**Q5. Why run tests in CI?**  
CI catches regressions before changes merge and gives the team shared confidence in the codebase.
