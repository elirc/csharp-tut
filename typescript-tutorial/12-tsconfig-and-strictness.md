# 12 - TSConfig and Strictness

`tsconfig.json` controls how TypeScript checks and emits your project.

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "ESNext",
    "strict": true,
    "noEmit": true
  },
  "include": ["src"]
}
```

Strictness is where TypeScript earns its keep. Turning strict checks off can make errors disappear, but it also makes TypeScript less protective.

Important strictness options include:

- `strict`
- `noImplicitAny`
- `strictNullChecks`
- `strictFunctionTypes`
- `noUncheckedIndexedAccess`
- `exactOptionalPropertyTypes`

Teams choose settings based on project maturity. For new learning projects, start strict.

## Exercises

1. Create a strict `tsconfig.json`.
2. Turn on `noUncheckedIndexedAccess`.
3. Observe how array indexing changes.
4. Set `noEmit` for type-check-only projects.
