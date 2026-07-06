# React Hooks and Context

Typing hooks and context in React 19 without `as` or non-null assertions.

## Activation

Load when writing React hooks, custom hooks, or context providers in `.tsx`/`.ts` files. Skip for non-React code.

## Rule

Keep state local with typed `useState`/`useReducer`/`useRef`, and give every context a provider-guard hook that throws when the provider is missing instead of asserting non-null.

## Why

The classic `createContext<T>(null!)` and `useContext(Ctx)!` patterns rely on `!`, which is banned. A default of `undefined` plus a guard hook is type-safe and fails loudly when a consumer is rendered outside its provider (a programmer bug).

## Do

- Name custom hooks `useXxx`; give exported hooks an explicit return type.
- Provide an explicit type argument when the initial value cannot infer it: `useState<string>()`, `useRef<HTMLInputElement | null>(null)`.
- Create context with `createContext<Value | undefined>(undefined)` and read it through a guard hook.
- Use the React 19 `<Context value={...}>` provider form directly.
- Keep state in the component that owns it; lift it up only when a sibling needs it.

## Avoid

- `null!` or `as` to paper over a context's initial value.
- `useContext` calls scattered across consumers without a guard hook.
- Lifting state higher than the nearest common owner.

## Example

```tsx
import { createContext, useContext } from "react";
import type { ReactNode } from "react";

interface AuthValue {
  readonly user: string;
}

interface AuthProviderProps {
  readonly user: string;
  readonly children: ReactNode;
}

const AuthContext = createContext<AuthValue | undefined>(undefined);

export function AuthProvider({ user, children }: AuthProviderProps): ReactNode {
  return <AuthContext value={{ user }}>{children}</AuthContext>;
}

export function useAuth(): AuthValue {
  const value = useContext(AuthContext);
  if (value === undefined) {
    throw new Error("useAuth must be used within AuthProvider");
  }
  return value;
}
```

## Exceptions

None. Component and props typing live in [react-components.md](react-components.md).
