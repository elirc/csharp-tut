# 08 - Keyof, Typeof, and Indexed Access Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `keyof User` | Union of property names. |
| 2 | `typeof config` | Type of a value. |
| 3 | `User["id"]` | Type of one property. |
| 4 | `User[keyof User]` | Union of all property value types. |
| 5 | `K extends keyof T` | Restricts K to valid keys of T. |
| 6 | `function get<T, K extends keyof T>(obj: T, key: K): T[K]` | Type-safe property access. |
| 7 | `const fields = ["id", "name"] as const` | Literal tuple of fields. |
| 8 | `typeof fields[number]` | Union of tuple element values. |
| 9 | `typeof import("./module")` | Type of a module. |
| 10 | `ReturnType<typeof fn>` | Return type of a function value. |
| 11 | `Parameters<typeof fn>` | Parameter tuple of a function value. |
| 12 | `InstanceType<typeof ClassName>` | Instance type from a class constructor. |
| 13 | `keyof typeof constants` | Keys from a runtime object. |
| 14 | `as const object` | Preserves literal object values. |
| 15 | `type EventName = keyof Handlers` | Derives valid event names. |
| 16 | `type ValueOf<T> = T[keyof T]` | Helper for union of object values. |
| 17 | `indexed access` | Reads types using bracket syntax. |
| 18 | `type duplication avoided` | Derive instead of copying types manually. |
| 19 | `property-safe APIs` | Prevent invalid key strings. |
| 20 | `rename-safe types` | Derived keys update during refactors. |
