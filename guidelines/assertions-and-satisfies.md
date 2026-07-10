# Assertions and satisfies

## Rule

Never use a type assertion (`as`) or a non-null assertion (`!`) in production code except `as const`; use `satisfies` to check a value against a type without widening it, and narrow instead of asserting non-null.

## Why

`as` and `!` override the checker and turn a type error into a runtime bug. `satisfies` gives the same intent, a value that conforms to a type, while keeping the narrow inferred type and still reporting mismatches.

## Do

- Use `satisfies` to validate object literals and preserve literal types.
- Use `as const` for immutable literals and to derive unions.
- Narrow with a guard or an explicit check to remove `undefined`, not `!`.
- Use a `const` type parameter (`<const T>`) to infer literal types on inputs.

## Example

```ts
interface Route {
  readonly path: string;
  readonly method: "GET" | "POST";
}

const routes = {
  home: { path: "/", method: "GET" },
  submit: { path: "/submit", method: "POST" },
} satisfies Record<string, Route>;

// routes.home.method stays "GET", not the wider string.

function firstMethod(list: readonly Route[]): string | undefined {
  const head = list[0];
  return head === undefined ? undefined : head.method;
}
```
