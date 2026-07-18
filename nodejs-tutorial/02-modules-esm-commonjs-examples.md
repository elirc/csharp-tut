# 02 - Modules: ESM and CommonJS Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `export function add() {}` | Named ES module export. |
| 2 | `import { add } from "./math.js"` | Named ES module import. |
| 3 | `export default value` | Default ES module export. |
| 4 | `import value from "./value.js"` | Default ES module import. |
| 5 | `import * as fs from "node:fs"` | Namespace import. |
| 6 | `await import("./plugin.js")` | Dynamic import. |
| 7 | `require("node:fs")` | CommonJS import. |
| 8 | `module.exports = value` | CommonJS export. |
| 9 | `exports.add = add` | CommonJS named-style export. |
| 10 | `.mjs` | Always ES module. |
| 11 | `.cjs` | Always CommonJS. |
| 12 | `"type": "module"` | Makes `.js` ESM in a package. |
| 13 | `"type": "commonjs"` | Makes `.js` CommonJS in a package. |
| 14 | `import.meta.url` | Current module URL in ESM. |
| 15 | `__dirname` | CommonJS directory variable. |
| 16 | `__filename` | CommonJS file variable. |
| 17 | `node:` prefix | Explicit built-in module import. |
| 18 | `exports field` | Controls package public entry points. |
| 19 | `default interop` | ESM/CommonJS compatibility edge area. |
| 20 | `module boundary` | File-level unit of code organization. |
| 21 | `import { readFile } from "node:fs/promises"` | Imports a named API from a built-in ESM module. |
| 22 | `const { readFile } = require("node:fs/promises")` | CommonJS destructuring import. |
| 23 | `export { add } from "./math.js"` | Re-exports from another module. |
| 24 | `export * from "./types.js"` | Re-exports all named exports. |
| 25 | `createRequire(import.meta.url)` | Allows `require` from an ES module. |
| 26 | `fileURLToPath(import.meta.url)` | Converts an ES module URL to a file path. |
| 27 | `import.meta.dirname` | Modern helper for current module directory where supported. |
| 28 | `package imports` | Internal package import aliases such as `#config`. |
| 29 | `conditional exports` | Exposes different entry points for different environments. |
| 30 | `bare specifier` | Import like `express`, resolved from packages. |

## Interview Questions

**Q1. What are the two major module systems in Node.js?**  
ES modules and CommonJS.

**Q2. How do you enable ES modules for `.js` files?**  
Set `"type": "module"` in `package.json`, or use the `.mjs` extension.

**Q3. What is the CommonJS equivalent of `import` and `export`?**  
`require()` imports values, and `module.exports` or `exports` exports values.

**Q4. Why can mixing ESM and CommonJS be tricky?**  
They have different loading behavior, syntax, default export interop, and file/package rules.

**Q5. Why use the `node:` prefix for built-in modules?**  
It makes it explicit that the import is a Node built-in module, not a package from `node_modules`.
