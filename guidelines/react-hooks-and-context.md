# React Hooks and Context

Hooks, effects, context, and shared state in React 19 without `as` or non-null assertions.

## Activation

Apply when writing React hooks, custom hooks, effects, or context providers in `.tsx`/`.ts` files. Skip for non-React code.

## Rule

Keep state local with typed `useState`/`useReducer`/`useRef`, give every context a provider-guard hook that throws when the provider is missing, and never call `useEffect` directly: derive values during render, act in event handlers, fetch with SWR, and route mount-only external sync through a single `useMountEffect` wrapper.

## Why

The classic `createContext<T>(null!)` and `useContext(Ctx)!` patterns rely on `!`, which is banned. A default of `undefined` plus a guard hook is type-safe and fails loudly when a consumer is rendered outside its provider (a programmer bug). Direct effects turn a component into a timeline of hidden steps a reader must replay; deriving values during render keeps it readable top to bottom (props in, values derived, JSX out). The React team's ["You Might Not Need an Effect"](https://react.dev/learn/you-might-not-need-an-effect) catalogues why most effects are bugs waiting to happen.

## Do

- Name custom hooks `useXxx`; give exported hooks an explicit return type.
- Provide an explicit type argument when the initial value cannot infer it: `useState<string>()`, `useRef<HTMLInputElement | null>(null)`.
- Create context with `createContext<Value | undefined>(undefined)` and read it through a guard hook.
- Use the React 19 `<Context value={...}>` provider form directly.
- Keep state in the component that owns it; lift it up only when a sibling needs it.
- Compute derived values during render; never mirror props or state into other state with an effect.
- Do work (POSTs, notifications, state updates) in the event handler that triggered it; reset per-entity state by changing the component's `key`.
- Fetch with SWR through named `useXxx` query hooks (and mutation helpers) in one central module per package.
- Wrap genuine mount-only external sync (third-party widgets, subscriptions, analytics) in the project's single `useMountEffect` helper, and ban direct imports with Biome `noRestrictedImports` (`useEffect`, `useLayoutEffect`) in frontend packages, excepting only that helper's module.
- Share state across deeply nested components with Zustand only after lifting state and context prove insufficient.

## Avoid

- `null!` or `as` to paper over a context's initial value.
- `useContext` calls scattered across consumers without a guard hook.
- Lifting state higher than the nearest common owner.
- Calling `useEffect` or `useLayoutEffect` directly in a component.
- `useEffect(() => setX(derive(y)), [y])` state mirroring, effect chains that trigger each other, and fetch-plus-`setState` effects (SWR owns caching, races, and revalidation).
- Zustand or context for state a single subtree owns.

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

The sole permitted `useEffect` call site:

```tsx
import { useEffect } from "react";

export function useMountEffect(effect: () => void | (() => void)): void {
  // biome-ignore lint/correctness/useExhaustiveDependencies: mount-only by design (REACT-3)
  useEffect(effect, []);
}
```

## Exceptions

An effect that is neither mount-only nor expressible as derivation, an event handler, or an SWR hook (for example, dependency-driven sync into an imperative widget) wraps `useEffect` in a purpose-named custom hook with a documented suppression; see [suppressions.md](suppressions.md). Component and props typing live in [react-components.md](react-components.md).
