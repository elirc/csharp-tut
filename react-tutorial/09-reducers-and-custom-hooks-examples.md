# 09 - Reducers and Custom Hooks Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `const [state, dispatch] = useReducer(reducer, initialState);` | Declares reducer state. |
| 2 | `dispatch({ type: "added", item })` | Sends an action to the reducer. |
| 3 | `function reducer(state, action) {}` | Defines state transition logic. |
| 4 | `switch (action.type)` | Common reducer branching pattern. |
| 5 | `return state;` | Leaves state unchanged for unknown actions. |
| 6 | `return [...state, item]` | Adds to array state immutably. |
| 7 | `return state.filter(x => x.id !== id)` | Removes from array state immutably. |
| 8 | `return state.map(...)` | Updates array items immutably. |
| 9 | `useReducer` | Useful when state transitions are named actions. |
| 10 | `useState` | Simpler for isolated state. |
| 11 | `function useToggle() {}` | Custom hook naming starts with `use`. |
| 12 | `const [open, toggleOpen] = useToggle();` | Consumes a custom hook. |
| 13 | `useLocalStorage(key, initial)` | Common reusable persistence hook. |
| 14 | `useFetch(url)` | Common reusable data-fetching hook. |
| 15 | `return { data, loading, error }` | Custom hooks can return objects. |
| 16 | `return [value, setValue]` | Custom hooks can return tuples. |
| 17 | `hooks call other hooks` | Custom hooks compose React behavior. |
| 18 | `top-level hook calls` | Hooks must not be called conditionally. |
| 19 | `reducer must be pure` | Reducers should not mutate or perform side effects. |
| 20 | `action payload` | Carries data needed for a transition. |
