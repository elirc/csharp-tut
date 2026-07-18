# 11 - Modules and Declaration Files Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `export type User = { id: string }` | Exports a type. |
| 2 | `export function add(...)` | Exports a runtime function. |
| 3 | `import { add } from "./math"` | Imports a runtime value. |
| 4 | `import type { User } from "./types"` | Imports only a type. |
| 5 | `export default class User {}` | Default export. |
| 6 | `export { add, subtract }` | Named export list. |
| 7 | `declare module "legacy-lib"` | Declares types for an untyped module. |
| 8 | `.d.ts` | Declaration file extension. |
| 9 | `declare global` | Adds global types. |
| 10 | `@types/node` | Node type declarations. |
| 11 | `@types/react` | React type declarations, often installed by tooling. |
| 12 | `types` in package.json | Points to package declaration file. |
| 13 | `type-only import` | Avoids unnecessary runtime import. |
| 14 | `namespace` | Older TypeScript organization feature. |
| 15 | `module resolution` | How TypeScript finds imported files. |
| 16 | `paths` | TSConfig option for import aliases. |
| 17 | `baseUrl` | Base folder for path mapping. |
| 18 | `isolatedModules` | Ensures files can compile independently. |
| 19 | `verbatimModuleSyntax` | Keeps import/export behavior explicit. |
| 20 | `ambient declaration` | Type declaration for something provided elsewhere. |
