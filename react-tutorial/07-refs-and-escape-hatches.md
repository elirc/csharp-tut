# 07 - Refs and Escape Hatches

Refs store values that persist between renders without causing a re-render.

```jsx
import { useRef } from "react";

function SearchBox() {
  const inputRef = useRef(null);

  return (
    <>
      <input ref={inputRef} />
      <button onClick={() => inputRef.current.focus()}>Focus</button>
    </>
  );
}
```

Refs are useful for DOM nodes, timer IDs, previous values, and integration with non-React code. Do not use refs to store ordinary UI state that should trigger rendering.

Refs are an escape hatch. Reach for them when React's normal state and props model does not fit.

## Exercises

1. Focus an input with a ref.
2. Store a timer id in a ref.
3. Track a previous prop value.
4. Replace a ref with state when the UI must update.
