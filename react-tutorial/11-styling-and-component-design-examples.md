# 11 - Styling and Component Design Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `import "./App.css";` | Imports plain CSS. |
| 2 | `className="button"` | Applies a CSS class. |
| 3 | `className={active ? "tab active" : "tab"}` | Builds conditional classes. |
| 4 | `style={{ color: "red" }}` | Uses inline style object. |
| 5 | `Button.module.css` | CSS module file convention. |
| 6 | `import styles from "./Button.module.css";` | Imports CSS module classes. |
| 7 | `className={styles.button}` | Applies a CSS module class. |
| 8 | `children` | Enables flexible composition. |
| 9 | `variant="primary"` | Common prop for visual variants. |
| 10 | `size="sm"` | Common prop for component scale. |
| 11 | `disabled={isSaving}` | Expresses disabled state. |
| 12 | `aria-busy={isLoading}` | Communicates loading state. |
| 13 | `role="alert"` | Announces important feedback. |
| 14 | `data-state="open"` | Styling hook for component states. |
| 15 | `component composition` | Combines small pieces into larger UI. |
| 16 | `presentational component` | Focuses on display rather than data fetching. |
| 17 | `container component` | Coordinates data and passes props down. |
| 18 | `design tokens` | Shared values for color, spacing, and typography. |
| 19 | `responsive CSS` | Adapts UI across screen sizes. |
| 20 | `accessible name` | Text or ARIA that identifies a control. |
