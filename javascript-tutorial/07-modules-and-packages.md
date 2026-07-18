# 07 - Modules and Packages

Modules split code across files. Modern JavaScript uses ES modules with `export` and `import`.

```js
// math.js
export function add(a, b) {
  return a + b;
}
```

```js
// index.js
import { add } from "./math.js";
console.log(add(2, 3));
```

Use default exports sparingly and intentionally. Named exports make refactoring and auto-imports clearer in many codebases.

Packages are reusable modules installed through npm. A package can be runtime code or development tooling.

```bash
npm install express
npm install -D vitest
```

Understand the difference between your code, installed dependencies, scripts, and generated lock files. Modern JavaScript development is inseparable from package tooling.

## Exercises

1. Create a module with two named exports.
2. Import those exports from another file.
3. Add an npm script that runs your entry file.
4. Install a package and use it.
