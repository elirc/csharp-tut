# 13 - Testing React Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `render(<App />)` | Renders a component in a test. |
| 2 | `screen.getByText("Save")` | Finds visible text. |
| 3 | `screen.getByRole("button")` | Finds by accessible role. |
| 4 | `getByRole("button", { name: /save/i })` | Finds by role and accessible name. |
| 5 | `userEvent.setup()` | Creates a user interaction helper. |
| 6 | `await user.click(button)` | Simulates a click. |
| 7 | `await user.type(input, "Ada")` | Simulates typing. |
| 8 | `expect(element).toBeInTheDocument()` | Asserts an element is present. |
| 9 | `expect(button).toBeDisabled()` | Asserts disabled state. |
| 10 | `queryByText("Error")` | Checks for optional absence. |
| 11 | `findByText("Loaded")` | Waits for async UI. |
| 12 | `waitFor(() => expect(...))` | Waits for an async assertion. |
| 13 | `vi.fn()` | Creates a mock callback. |
| 14 | `expect(onSave).toHaveBeenCalled()` | Checks callback behavior. |
| 15 | `MemoryRouter` | Tests routed components. |
| 16 | `mock fetch` | Controls network responses in tests. |
| 17 | `MSW` | Mock Service Worker for API-level mocks. |
| 18 | `beforeEach` | Sets up repeated test state. |
| 19 | `afterEach` | Cleans up mocks or timers. |
| 20 | `test behavior` | Prefer user-observable behavior over internals. |
