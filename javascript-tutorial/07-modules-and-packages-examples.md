# 07 - Modules and Packages Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `export const port = 3000;` | Exports a named value. |
| 2 | `export function add(a, b) {}` | Exports a named function. |
| 3 | `import { add } from "./math.js";` | Imports a named export. |
| 4 | `export default class User {}` | Exports one default value. |
| 5 | `import User from "./User.js";` | Imports a default export. |
| 6 | `import * as math from "./math.js";` | Imports a module namespace object. |
| 7 | `await import("./plugin.js")` | Dynamically imports a module. |
| 8 | `"type": "module"` | Tells Node to treat `.js` files as ES modules. |
| 9 | `npm install express` | Installs a runtime dependency. |
| 10 | `npm install -D eslint` | Installs a dev dependency. |
| 11 | `dependencies` | Packages needed by the app at runtime. |
| 12 | `devDependencies` | Packages used for development tasks. |
| 13 | `npm run lint` | Runs a project script. |
| 14 | `npx vite` | Runs a package binary without a permanent script. |
| 15 | `package-lock.json` | Records resolved dependency versions. |
| 16 | `node_modules` | Contains installed package files. |
| 17 | `import fs from "node:fs";` | Imports a Node built-in module. |
| 18 | `import path from "node:path";` | Imports path utilities. |
| 19 | `export { add, subtract };` | Exports several names at once. |
| 20 | `import { add as sum } from "./math.js";` | Renames an import locally. |
