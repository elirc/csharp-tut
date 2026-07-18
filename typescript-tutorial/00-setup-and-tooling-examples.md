# 00 - Setup and Tooling Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `npm install -D typescript` | Installs TypeScript as a dev dependency. |
| 2 | `npx tsc --init` | Creates a `tsconfig.json` file. |
| 3 | `npx tsc` | Runs the TypeScript compiler. |
| 4 | `npx tsc --watch` | Recompiles when files change. |
| 5 | `src/index.ts` | Common TypeScript source file path. |
| 6 | `dist/` | Common compiled output folder. |
| 7 | `"strict": true` | Enables strong type checking. |
| 8 | `"target": "ES2022"` | Chooses JavaScript output language level. |
| 9 | `"module": "ESNext"` | Chooses module output style. |
| 10 | `"moduleResolution": "Bundler"` | Common setting for Vite-style projects. |
| 11 | `"noEmit": true` | Type-checks without writing output files. |
| 12 | `"outDir": "dist"` | Sends compiler output to a folder. |
| 13 | `"rootDir": "src"` | Declares the source root folder. |
| 14 | `"include": ["src"]` | Limits files included by the project. |
| 15 | `.ts` | TypeScript file extension. |
| 16 | `.tsx` | TypeScript file extension for JSX. |
| 17 | `.d.ts` | Declaration file containing types only. |
| 18 | `ts-node` | Tooling option for running TS directly. |
| 19 | `tsx` | Popular fast TS runner for Node scripts. |
| 20 | `TypeScript Playground` | Browser tool for experimenting with TS. |
