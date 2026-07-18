# 05 - Forms and Controlled Inputs Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `const [name, setName] = useState("");` | Stores input text. |
| 2 | `<input value={name} />` | Makes an input controlled. |
| 3 | `onChange={e => setName(e.target.value)}` | Updates state from input changes. |
| 4 | `event.preventDefault()` | Prevents page reload on submit. |
| 5 | `<form onSubmit={handleSubmit}>` | Handles form submission. |
| 6 | `<label htmlFor="email">Email</label>` | Labels an input accessibly. |
| 7 | `<input id="email" />` | Matches a label with `htmlFor`. |
| 8 | `checked={accepted}` | Controls a checkbox. |
| 9 | `onChange={e => setAccepted(e.target.checked)}` | Reads checkbox state. |
| 10 | `<select value={role}>` | Controls a select input. |
| 11 | `<textarea value={notes}>` | Controls a textarea. |
| 12 | `required` | Adds browser-level required validation. |
| 13 | `disabled={isSaving}` | Prevents interaction while saving. |
| 14 | `{error && <p role="alert">{error}</p>}` | Shows accessible validation feedback. |
| 15 | `name.trim()` | Normalizes text input. |
| 16 | `Number(event.target.value)` | Converts numeric input text. |
| 17 | `setForm({ ...form, title })` | Updates one field in object state. |
| 18 | `[event.target.name]: event.target.value` | Updates fields dynamically by input name. |
| 19 | `defaultValue="Ada"` | Sets an uncontrolled initial value. |
| 20 | `new FormData(event.currentTarget)` | Reads form values from the DOM. |
