# 07 - Generics Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `function identity<T>(value: T): T` | Preserves caller-chosen type. |
| 2 | `function first<T>(items: T[]): T` | Returns array element type. |
| 3 | `Array<T>` | Generic array type. |
| 4 | `Promise<T>` | Generic async result type. |
| 5 | `Record<K, V>` | Generic key/value object type. |
| 6 | `type Result<T> = { ok: true; value: T }` | Generic result wrapper. |
| 7 | `<T extends { id: string }>` | Constraint requiring an id. |
| 8 | `<K extends keyof T>` | Constraint to keys of another type. |
| 9 | `function get<T, K extends keyof T>(obj: T, key: K)` | Type-safe property getter. |
| 10 | `T[K]` | Indexed access type. |
| 11 | `Map<string, User>` | Generic collection type. |
| 12 | `Set<number>` | Generic set type. |
| 13 | `useState<User \| null>` | Generic React hook usage. |
| 14 | `Response<T>` | Common generic API response shape. |
| 15 | `default type parameter` | Provides fallback generic type. |
| 16 | `<T = unknown>` | Default generic type parameter. |
| 17 | `generic interface` | Interface with type parameters. |
| 18 | `generic class` | Class with type parameters. |
| 19 | `infer from argument` | Compiler chooses generic type from input. |
| 20 | `avoid fake generics` | Do not use `T` when a concrete type is clearer. |
