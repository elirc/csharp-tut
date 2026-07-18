# 02 - Modules: ESM and CommonJS

Node supports two module systems: ES modules and CommonJS.

ES module:

```js
// math.js
export function add(a, b) {
  return a + b;
}
```

```js
import { add } from "./math.js";
```

CommonJS:

```js
const fs = require("node:fs");
module.exports = { read };
```

Modern code often uses ES modules, especially when `"type": "module"` is set in `package.json`. Many older packages and codebases still use CommonJS.

Understand file extensions: `.mjs` is ESM, `.cjs` is CommonJS, and `.js` depends on the package `"type"` field.

## Exercises

1. Create an ES module export and import it.
2. Create a CommonJS module and require it.
3. Set `"type": "module"`.
4. Use a dynamic import.
5. Explain `.js`, `.mjs`, and `.cjs`.
