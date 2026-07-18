# 06 - Arrays, Tuples, and Readonly

Arrays describe a sequence of values with the same element type.

```ts
const names: string[] = ["Ada", "Grace"];
const scores: Array<number> = [95, 88];
```

Tuples describe arrays with fixed positions and known element types.

```ts
type Point = [number, number];
const point: Point = [10, 20];
```

Named tuple elements improve readability:

```ts
type Range = [start: number, end: number];
```

Readonly arrays prevent mutation through that reference:

```ts
function total(values: readonly number[]) {
  return values.reduce((sum, value) => sum + value, 0);
}
```

Use readonly parameters when a function should not mutate its inputs.

## Exercises

1. Type an array of strings.
2. Create a tuple for coordinates.
3. Use a readonly array parameter.
4. Convert a mutable update to an immutable copy.
