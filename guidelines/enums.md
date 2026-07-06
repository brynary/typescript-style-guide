# Enums

## Rule

Never use `enum` or `const enum`; model closed sets as a literal union, and when you also need the runtime values, define an `as const` object and derive the union from it.

## Why

`enum` is not erasable syntax, so it is banned under the toolchain baseline. Literal unions and `as const` objects are plain values, are fully type-checked, and tree-shake cleanly.

## Do

- Use a literal union when only the type is needed: `type Status = "open" | "closed"`.
- Use an `as const` object when the runtime values matter, then derive the union.
- Derive the union with `type X = (typeof Xs)[keyof typeof Xs]`.

## Avoid

- `enum Status { Open, Closed }` or `const enum`.
- Re-declaring the member list in both an object and a hand-written union.

## Example

```ts
const Status = {
  Open: "open",
  Closed: "closed",
} as const;

type Status = (typeof Status)[keyof typeof Status];

function label(status: Status): string {
  return status === Status.Open ? "Open" : "Closed";
}

// Type-only closed set, no runtime values needed:
type Direction = "north" | "south" | "east" | "west";
```

## Exceptions

None. This cascades from `erasableSyntaxOnly`; see [tsconfig-baseline](tsconfig-baseline.md).
