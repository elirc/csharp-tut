# 11 - Modules and Declaration Files

TypeScript uses JavaScript modules with typed imports and exports.

```ts
// math.ts
export function add(a: number, b: number): number {
  return a + b;
}
```

```ts
// index.ts
import { add } from "./math";
```

Declaration files (`.d.ts`) describe types for JavaScript code or package APIs without containing runtime implementation.

```ts
declare function analyticsTrack(eventName: string): void;
```

Many packages include their own types. Others use `@types` packages:

```bash
npm install -D @types/node
```

Understand the difference between runtime imports and type-only imports.

```ts
import type { User } from "./types";
```

## Exercises

1. Export and import a typed function.
2. Use `import type`.
3. Install `@types/node`.
4. Write a small `.d.ts` declaration.
