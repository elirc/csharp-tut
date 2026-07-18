# 14 - Testing JavaScript Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `import { it, expect } from "vitest";` | Imports test helpers. |
| 2 | `it("works", () => {})` | Defines a test case. |
| 3 | `describe("group", () => {})` | Groups related tests. |
| 4 | `expect(value).toBe(3)` | Checks strict equality. |
| 5 | `expect(obj).toEqual({ id: 1 })` | Checks object structure. |
| 6 | `expect(list).toContain("a")` | Checks array membership. |
| 7 | `expect(fn).toThrow()` | Checks that a function throws. |
| 8 | `await expect(promise).resolves.toBe(1)` | Tests a resolving promise. |
| 9 | `await expect(promise).rejects.toThrow()` | Tests a rejecting promise. |
| 10 | `beforeEach(() => {})` | Runs setup before each test. |
| 11 | `afterEach(() => {})` | Runs cleanup after each test. |
| 12 | `vi.fn()` | Creates a mock function. |
| 13 | `expect(mock).toHaveBeenCalled()` | Checks a mock was called. |
| 14 | `expect(mock).toHaveBeenCalledWith(id)` | Checks call arguments. |
| 15 | `vi.mock("./api.js")` | Mocks a module. |
| 16 | `test.each([[1, 2, 3]])` | Runs a parameterized test. |
| 17 | `npm test` | Common script for running tests. |
| 18 | `vitest --watch` | Reruns tests during development. |
| 19 | `vitest --coverage` | Measures test coverage when configured. |
| 20 | `supertest(app)` | Common way to integration-test Express APIs. |
