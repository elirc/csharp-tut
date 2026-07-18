# 12 - Performance and Rendering Behavior Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `state update triggers render` | Normal React behavior. |
| 2 | `props change triggers render` | Child components render when parent renders with new props. |
| 3 | `React.memo(Component)` | Skips rendering when props are equal. |
| 4 | `useMemo(() => compute(), [items])` | Memoizes an expensive calculated value. |
| 5 | `useCallback(fn, [deps])` | Memoizes a function identity. |
| 6 | `key changes reset state` | React treats a new key as a new component. |
| 7 | `state close to usage` | Reduces unnecessary parent and sibling renders. |
| 8 | `derived data during render` | Avoids extra state and effects. |
| 9 | `virtualized list` | Renders only visible rows for huge lists. |
| 10 | `pagination` | Limits data and DOM work. |
| 11 | `debounce search` | Reduces frequent expensive updates. |
| 12 | `transition` | Marks non-urgent UI updates. |
| 13 | `useDeferredValue` | Defers rendering a less urgent value. |
| 14 | `Profiler` | Measures rendering cost. |
| 15 | `production build` | Required for meaningful performance checks. |
| 16 | `stable keys` | Help React preserve list item identity. |
| 17 | `avoid inline heavy work` | Move expensive calculation behind memoization if needed. |
| 18 | `avoid premature memo` | Memoization has complexity and overhead. |
| 19 | `network waterfall` | Often bigger problem than render speed. |
| 20 | `measure first` | Optimize based on evidence. |
