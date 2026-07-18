# 13 - Testing React

React tests should focus on behavior users can observe: text, buttons, form input, navigation, loading, errors, and results.

React Testing Library encourages testing through the DOM:

```jsx
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";

test("increments the counter", async () => {
  const user = userEvent.setup();
  render(<Counter />);

  await user.click(screen.getByRole("button", { name: /add/i }));

  expect(screen.getByText("1")).toBeInTheDocument();
});
```

Avoid testing implementation details such as state variable names. Test what the user sees and does.

## Exercises

1. Test that a component renders text.
2. Test a button click.
3. Test a controlled input.
4. Test loading and error states.
