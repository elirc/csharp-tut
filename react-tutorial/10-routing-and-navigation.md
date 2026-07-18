# 10 - Routing and Navigation

React itself does not include routing. Many apps use React Router or a framework with routing built in.

With React Router, routes map URLs to components:

```jsx
import { createBrowserRouter, RouterProvider } from "react-router-dom";

const router = createBrowserRouter([
  { path: "/", element: <HomePage /> },
  { path: "/tasks/:id", element: <TaskPage /> }
]);

export function App() {
  return <RouterProvider router={router} />;
}
```

Routing is more than changing screens. It includes links, route params, query strings, not-found pages, loading states, protected routes, and preserving useful state in the URL.

## Exercises

1. Add two routes.
2. Link between pages.
3. Read a route parameter.
4. Add a not-found page.
