# 08 - Sharing State and Context Examples

| # | Example | Explanation |
|---|---|---|
| 1 | `const [selectedId, setSelectedId] = useState(null);` | Stores shared selection in a parent. |
| 2 | `<List selectedId={selectedId} />` | Passes shared state down. |
| 3 | `<List onSelect={setSelectedId} />` | Passes a state updater down. |
| 4 | `closest common parent` | Best home for state used by siblings. |
| 5 | `createContext(defaultValue)` | Creates a context object. |
| 6 | `useContext(ThemeContext)` | Reads context in a component. |
| 7 | `<ThemeContext value="dark">` | Provides context in modern React. |
| 8 | `<ThemeContext.Provider value="dark">` | Provider form used across many codebases. |
| 9 | `currentUser context` | Good candidate for app-wide data. |
| 10 | `theme context` | Good candidate for visual setting data. |
| 11 | `locale context` | Good candidate for language setting data. |
| 12 | `modal open state` | Often belongs near the modal owner, not global context. |
| 13 | `prop drilling` | Passing props through layers that only forward them. |
| 14 | `children composition` | Alternative to passing many props through wrappers. |
| 15 | `value={{ user, logout }}` | Provides data and behavior together. |
| 16 | `useMemo for context value` | Can reduce unnecessary context value identity changes. |
| 17 | `split contexts` | Separates unrelated values. |
| 18 | `server state` | Usually better handled by data-fetching tools than context. |
| 19 | `local state first` | Keep state close until sharing is needed. |
| 20 | `lift state intentionally` | Move state only when another component truly needs it. |
