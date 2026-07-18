# 05 - Forms and Controlled Inputs

Controlled inputs store form values in React state.

```jsx
function NameForm() {
  const [name, setName] = useState("");

  function handleSubmit(event) {
    event.preventDefault();
    console.log(name);
  }

  return (
    <form onSubmit={handleSubmit}>
      <input value={name} onChange={(event) => setName(event.target.value)} />
      <button>Save</button>
    </form>
  );
}
```

Forms are where UI state, validation, user feedback, and API calls meet. Start simple: one state value per input. As forms grow, consider reducers, form libraries, or schema validation.

Always label inputs. Good form code is not only about state; it is also about accessibility and clear feedback.

## Exercises

1. Build a controlled text input.
2. Add a checkbox.
3. Validate required fields.
4. Disable submit while saving.
