# 14 - Runtime Validation and API Boundaries Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `const data: unknown = await response.json()` | Treats external JSON as untrusted. |
| 2 | `value is User` | Return type for a custom type guard. |
| 3 | `asserts value is User` | Assertion function return type. |
| 4 | `typeof value === "object"` | First step for object validation. |
| 5 | `value !== null` | Excludes null object quirk. |
| 6 | `"id" in value` | Checks property presence. |
| 7 | `typeof value.id === "string"` | Checks property type. |
| 8 | `Array.isArray(value)` | Validates arrays. |
| 9 | `value.every(isUser)` | Validates array elements. |
| 10 | `as User` | Assertion, not validation. |
| 11 | `JSON.parse(text) as unknown` | Parsed data should be treated as unknown. |
| 12 | `process.env.API_URL` | Environment values may be undefined. |
| 13 | `if (!url) throw new Error(...)` | Runtime config validation. |
| 14 | `Zod schema` | Popular runtime validation approach. |
| 15 | `z.infer<typeof Schema>` | Derives TypeScript type from Zod schema. |
| 16 | `safeParse` | Validation that returns success/failure. |
| 17 | `API boundary` | Place where typed code meets untrusted data. |
| 18 | `DTO` | Shape used for data transfer. |
| 19 | `domain type` | Shape used inside business logic. |
| 20 | `validate at edges` | Keep unsafe data from spreading through the app. |
