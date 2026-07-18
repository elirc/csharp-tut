# 07 - Refs and Escape Hatches Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `const inputRef = useRef(null);` | Creates a ref object. |
| 2 | `<input ref={inputRef} />` | Connects a DOM node to a ref. |
| 3 | `inputRef.current.focus()` | Imperatively focuses an input. |
| 4 | `const idRef = useRef(null);` | Stores a timer id. |
| 5 | `idRef.current = setTimeout(...)` | Updates a ref without rendering. |
| 6 | `clearTimeout(idRef.current)` | Cleans up stored timer id. |
| 7 | `ref.current` | Reads or writes the current ref value. |
| 8 | `useRef(0)` | Initializes a ref value. |
| 9 | `renderCount.current += 1` | Tracks information across renders. |
| 10 | `previous.current = value` | Stores a previous value. |
| 11 | `useImperativeHandle` | Customizes a ref exposed by a component. |
| 12 | `forwardRef` | Allows a component to receive a ref. |
| 13 | `videoRef.current.play()` | Controls a media element. |
| 14 | `dialogRef.current.showModal()` | Calls an imperative DOM API. |
| 15 | `canvasRef.current.getContext("2d")` | Integrates with canvas. |
| 16 | `state causes render` | State is for render-driving data. |
| 17 | `ref does not cause render` | Refs are for non-render data. |
| 18 | `read ref in event handler` | Safe place to use current ref values. |
| 19 | `avoid ref for derived UI` | Derived UI should come from state or props. |
| 20 | `cleanup external library` | Refs help manage third-party instances. |
