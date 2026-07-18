# 05 - Object Types and Interfaces Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `type User = { id: string }` | Object type alias. |
| 2 | `interface User { id: string }` | Object interface. |
| 3 | `email?: string` | Optional property. |
| 4 | `readonly id: string` | Read-only property. |
| 5 | `{ x: number; y: number }` | Inline object type. |
| 6 | `Record<string, number>` | String-keyed object values. |
| 7 | `{ [key: string]: number }` | Index signature. |
| 8 | `type Admin = User & { role: "admin" }` | Intersection type. |
| 9 | `interface Admin extends User {}` | Interface extension. |
| 10 | `method(): void` | Method property in an object type. |
| 11 | `onClick: () => void` | Function property. |
| 12 | `nested: { name: string }` | Nested object type. |
| 13 | `excess property check` | Object literals get extra checks. |
| 14 | `structural compatibility` | Shape determines assignment compatibility. |
| 15 | `type merge impossible` | Type aliases do not declaration-merge. |
| 16 | `interface merging` | Interfaces can merge declarations. |
| 17 | `Pick<User, "id">` | Selects part of an object type. |
| 18 | `Omit<User, "email">` | Removes keys from an object type. |
| 19 | `Partial<User>` | Makes properties optional. |
| 20 | `Required<User>` | Makes properties required. |
