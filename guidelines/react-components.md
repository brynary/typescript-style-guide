# React Components

Component and props typing for React 19 with TSX.

## Activation

Apply when writing or editing React components or `.tsx` files. Skip for non-React code.

## Rule

Write components as plain function declarations that take a single typed props parameter, never `React.FC`.

## Why

A plain function with an explicit props interface keeps the component contract visible and works cleanly with generics. React 19 removed the need for `forwardRef`, so a `ref` is just another prop.

## Do

- Name the props interface `XxxProps` for component `Xxx` and mark every member `readonly`.
- Destructure props in the parameter list and annotate the return type as `ReactNode`.
- Type `children` explicitly as `ReactNode` when the component renders them.
- Accept `ref` as a regular optional prop typed `Ref<T>` (no `forwardRef`).
- Style with Tailwind CSS utility classes in `className`.

## Avoid

- `React.FC`, `React.FunctionComponent`, or `forwardRef`.
- Inline object-literal prop types; declare a named `XxxProps` interface instead.
- Fetching data or wiring shared state in the component body; use the SWR query hooks and state conventions in [react-hooks-and-context.md](react-hooks-and-context.md).
- CSS-in-JS or ad hoc stylesheets in place of Tailwind classes.

## Example

```tsx
import type { ReactNode, Ref } from "react";

interface ButtonProps {
  readonly label: string;
  readonly onClick: () => void;
  readonly ref?: Ref<HTMLButtonElement>;
  readonly children?: ReactNode;
}

export function Button({ label, onClick, ref, children }: ButtonProps): ReactNode {
  return (
    <button
      ref={ref}
      type="button"
      onClick={onClick}
      className="rounded bg-blue-600 px-4 py-2 text-white"
    >
      {label}
      {children}
    </button>
  );
}
```
