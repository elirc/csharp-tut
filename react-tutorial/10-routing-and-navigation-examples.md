# 10 - Routing and Navigation Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `npm install react-router-dom` | Adds React Router. |
| 2 | `createBrowserRouter(routes)` | Creates a browser router. |
| 3 | `<RouterProvider router={router} />` | Mounts the router. |
| 4 | `{ path: "/", element: <Home /> }` | Defines a route. |
| 5 | `{ path: "/tasks/:id", element: <Task /> }` | Defines a dynamic segment. |
| 6 | `useParams()` | Reads route parameters. |
| 7 | `useNavigate()` | Navigates from code. |
| 8 | `<Link to="/tasks">Tasks</Link>` | Navigates without full page reload. |
| 9 | `<NavLink to="/tasks">Tasks</NavLink>` | Link with active state support. |
| 10 | `useSearchParams()` | Reads and writes query string state. |
| 11 | `<Outlet />` | Renders nested route children. |
| 12 | `errorElement` | Route-level error UI. |
| 13 | `loader` | Route-level data loading in data routers. |
| 14 | `action` | Route-level mutation handler in data routers. |
| 15 | `navigate("/login")` | Imperative navigation. |
| 16 | `replace: true` | Replaces history entry during navigation. |
| 17 | `path: "*"` | Catch-all not-found route. |
| 18 | `protected route` | Checks auth before showing a screen. |
| 19 | `URL state` | Useful for filters, tabs, pages, and search. |
| 20 | `layout route` | Shares navigation or shell UI across pages. |
