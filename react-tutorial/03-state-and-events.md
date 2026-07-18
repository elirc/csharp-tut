# 03 - State and Events

State lets a component remember information between renders.

```jsx
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount(count + 1)}>{count}</button>;
}
```

Events are how users interact with UI. React uses camelCase event props such as `onClick`, `onChange`, and `onSubmit`.

When the next state depends on the previous state, use an updater function:

```jsx
setCount((current) => current + 1);
```

Do not mutate state directly. Create new arrays and objects when updating state.

## Exercises

1. Build a counter.
2. Toggle a boolean.
3. Add an item to an array state.
4. Update an object state without mutation.
