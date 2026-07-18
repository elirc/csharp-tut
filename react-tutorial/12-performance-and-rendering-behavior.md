# 12 - Performance and Rendering Behavior

React re-renders components when state changes. Re-rendering is normal. Do not optimize every render. First make the UI correct, then measure if it feels slow.

Common performance tools include `React.memo`, `useMemo`, and `useCallback`, but they are not magic. They help only when identity changes or expensive calculations cause real problems.

```jsx
const visibleTasks = useMemo(() => {
  return tasks.filter((task) => task.status === filter);
}, [tasks, filter]);
```

The best performance improvements often come from better state placement, smaller component boundaries, pagination, list virtualization, avoiding unnecessary effects, and not fetching the same data repeatedly.

## Exercises

1. Use React DevTools Profiler.
2. Move state down and observe fewer re-renders.
3. Memoize an expensive calculation.
4. Remove unnecessary memoization and confirm nothing gets worse.
