# 09 - Utility, Mapped, and Conditional Types Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `Partial<User>` | Makes all properties optional. |
| 2 | `Required<User>` | Makes all properties required. |
| 3 | `Readonly<User>` | Makes all properties readonly. |
| 4 | `Pick<User, "id">` | Keeps selected properties. |
| 5 | `Omit<User, "password">` | Removes selected properties. |
| 6 | `Record<string, number>` | Creates a key/value object type. |
| 7 | `Exclude<Status, "draft">` | Removes union members. |
| 8 | `Extract<Status, "open">` | Keeps matching union members. |
| 9 | `NonNullable<T>` | Removes null and undefined. |
| 10 | `ReturnType<typeof fn>` | Gets function return type. |
| 11 | `Parameters<typeof fn>` | Gets function parameter tuple. |
| 12 | `{ [K in keyof T]: T[K] }` | Basic mapped type. |
| 13 | `{ readonly [K in keyof T]: T[K] }` | Adds readonly modifier. |
| 14 | `{ [K in keyof T]?: T[K] }` | Adds optional modifier. |
| 15 | `{ [K in keyof T as `get${Capitalize<string & K>}`]: () => T[K] }` | Remaps keys. |
| 16 | `T extends string ? A : B` | Conditional type. |
| 17 | `T extends Array<infer U> ? U : T` | Infers array element type. |
| 18 | `Awaited<Promise<T>>` | Unwraps promise-like types. |
| 19 | `template literal type` | Builds string types from other string types. |
| 20 | `keep types readable` | Complex types should earn their complexity. |
