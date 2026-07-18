# 06 - Effects and Data Fetching

Effects synchronize a component with something outside React: network requests, browser APIs, subscriptions, timers, or third-party widgets.

```jsx
import { useEffect, useState } from "react";

function Users() {
  const [users, setUsers] = useState([]);

  useEffect(() => {
    let ignore = false;

    async function load() {
      const response = await fetch("/api/users");
      const data = await response.json();
      if (!ignore) setUsers(data);
    }

    load();
    return () => {
      ignore = true;
    };
  }, []);

  return <UserList users={users} />;
}
```

Effects are often overused. If you can calculate something during render from props or state, do that instead of using an effect. Use effects for synchronization, not ordinary data derivation.

## Exercises

1. Fetch data when a component mounts.
2. Add loading and error state.
3. Clean up a timer effect.
4. Remove an unnecessary effect by calculating during render.
