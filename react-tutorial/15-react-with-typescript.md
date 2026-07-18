# 15 - React With TypeScript

Many React teams use TypeScript. TypeScript helps document props, catch mistakes, and make refactoring safer.

```tsx
type ButtonProps = {
  children: React.ReactNode;
  disabled?: boolean;
  onClick: () => void;
};

function Button({ children, disabled = false, onClick }: ButtonProps) {
  return (
    <button disabled={disabled} onClick={onClick}>
      {children}
    </button>
  );
}
```

TypeScript does not remove the need for runtime validation. API responses and user input still need checks.

Learn prop types, event types, state types, refs, unions, generics, and how to type custom hooks.

## Exercises

1. Type a component's props.
2. Type a click handler.
3. Type `useState` with nullable data.
4. Type a custom hook return value.
