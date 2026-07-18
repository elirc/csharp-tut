# 08 - DOM and Browser Events Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `document.querySelector("#app")` | Selects the first matching element. |
| 2 | `document.querySelectorAll("li")` | Selects all matching elements. |
| 3 | `element.textContent = "Hi"` | Sets text content safely. |
| 4 | `element.innerHTML = "<strong>Hi</strong>"` | Sets HTML and must be used carefully. |
| 5 | `element.classList.add("active")` | Adds a CSS class. |
| 6 | `element.classList.toggle("open")` | Toggles a CSS class. |
| 7 | `element.dataset.id` | Reads a `data-id` attribute. |
| 8 | `document.createElement("li")` | Creates a new element. |
| 9 | `parent.append(child)` | Appends a child element. |
| 10 | `element.remove()` | Removes an element from the DOM. |
| 11 | `button.addEventListener("click", handler)` | Subscribes to an event. |
| 12 | `button.removeEventListener("click", handler)` | Unsubscribes from an event. |
| 13 | `event.preventDefault()` | Stops default browser behavior. |
| 14 | `event.target` | The element where the event originated. |
| 15 | `event.currentTarget` | The element handling the event. |
| 16 | `input.value` | Reads or writes an input value. |
| 17 | `form.addEventListener("submit", onSubmit)` | Handles form submission. |
| 18 | `localStorage.setItem("theme", "dark")` | Stores simple browser data. |
| 19 | `localStorage.getItem("theme")` | Reads simple browser data. |
| 20 | `window.location.href` | Reads or changes the current URL. |
