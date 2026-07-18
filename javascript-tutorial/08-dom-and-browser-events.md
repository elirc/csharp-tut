# 08 - DOM and Browser Events

In browsers, JavaScript can read and change the page through the DOM. The DOM represents HTML as objects.

```js
const heading = document.querySelector("h1");
heading.textContent = "Updated";
```

Use events to respond to user actions:

```js
const button = document.querySelector("button");
button.addEventListener("click", () => {
  console.log("Clicked");
});
```

DOM work has three recurring concerns: selecting elements, updating state, and keeping UI behavior predictable. Avoid scattering unrelated DOM updates everywhere. As apps grow, frameworks such as React, Vue, or Svelte help manage this complexity, but the DOM basics still matter.

## Exercises

1. Select an element and change its text.
2. Add a click handler to a button.
3. Read a value from an input.
4. Create and append a new list item.
