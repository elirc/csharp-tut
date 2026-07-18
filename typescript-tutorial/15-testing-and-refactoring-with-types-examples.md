# 15 - Testing and Refactoring With Types Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `expect(add(2, 3)).toBe(5)` | Runtime behavior test. |
| 2 | `@ts-expect-error` | Expects a compiler error on the next line. |
| 3 | `@ts-ignore` | Suppresses an error; avoid in most cases. |
| 4 | `rename symbol` | Editor refactor powered by types. |
| 5 | `find references` | Type-aware navigation. |
| 6 | `go to definition` | Jump to source of a type or value. |
| 7 | `exhaustive switch` | Compiler confirms all union cases handled. |
| 8 | `never check` | Fails when a union case is missing. |
| 9 | `type test` | Test that types accept or reject shapes. |
| 10 | `tsc --noEmit` | Type-checks in CI. |
| 11 | `lint` | Catches style and risky patterns. |
| 12 | `strict CI` | Prevents type errors from merging. |
| 13 | `public API type` | Type that other code depends on. |
| 14 | `breaking type change` | Change that requires call-site updates. |
| 15 | `safe refactor` | Compiler guides necessary edits. |
| 16 | `tests still needed` | Types do not verify business behavior. |
| 17 | `mock typed dependency` | Mock object must satisfy expected contract. |
| 18 | `satisfies` | Checks fixture shape without losing literals. |
| 19 | `fixture type` | Shared type for test data. |
| 20 | `coverage plus types` | Different tools protect different risks. |
