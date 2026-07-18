# 00 - Setup and Tooling

TypeScript is a language and compiler that checks your code before it runs. The compiler is usually called `tsc`.

Create a small project:

```bash
mkdir typed-app
cd typed-app
npm init -y
npm install -D typescript
npx tsc --init
```

Create `src/index.ts`:

```ts
const message: string = "Hello, TypeScript";
console.log(message);
```

Compile it:

```bash
npx tsc
```

TypeScript projects are shaped by `tsconfig.json`. This file controls strictness, module behavior, emitted output, included files, and JavaScript interop.

During learning, keep `strict` on. Strict mode makes TypeScript more useful because it forces you to handle uncertainty instead of accidentally ignoring it.

## Exercises

1. Create a TypeScript project.
2. Add `src/index.ts`.
3. Compile with `npx tsc`.
4. Turn on `strict` if it is not already enabled.
5. Create a type error and read the compiler message.
