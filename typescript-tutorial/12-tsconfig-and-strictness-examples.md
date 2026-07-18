# 12 - TSConfig and Strictness Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `"strict": true` | Enables all strict checks. |
| 2 | `"noImplicitAny": true` | Rejects implicit `any`. |
| 3 | `"strictNullChecks": true` | Makes null and undefined meaningful. |
| 4 | `"noUncheckedIndexedAccess": true` | Array/object indexing may return undefined. |
| 5 | `"exactOptionalPropertyTypes": true` | Treats optional properties more precisely. |
| 6 | `"noEmit": true` | Type-checks without output. |
| 7 | `"declaration": true` | Emits `.d.ts` files. |
| 8 | `"sourceMap": true` | Emits source maps. |
| 9 | `"target": "ES2022"` | Output JavaScript language level. |
| 10 | `"lib": ["DOM", "ES2022"]` | Available global APIs. |
| 11 | `"module": "NodeNext"` | Node ESM-aware module mode. |
| 12 | `"moduleResolution": "NodeNext"` | Node ESM-aware resolution. |
| 13 | `"moduleResolution": "Bundler"` | Bundler-oriented resolution. |
| 14 | `"jsx": "react-jsx"` | JSX transform for React. |
| 15 | `"allowJs": true` | Includes JavaScript files. |
| 16 | `"checkJs": true` | Type-checks JavaScript files. |
| 17 | `"skipLibCheck": true` | Skips checking declaration files. |
| 18 | `"baseUrl": "."` | Base path for module resolution. |
| 19 | `"paths": { "@/*": ["src/*"] }` | Import alias mapping. |
| 20 | `"include": ["src"]` | Project file inclusion. |
