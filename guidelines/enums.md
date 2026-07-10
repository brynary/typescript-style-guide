# Enums

## Rule

Never use `enum` or `const enum`; model closed sets as a literal union, and when you also need the runtime values, define an `as const` object and derive the union from it.

## Why

`enum` is not erasable syntax, so it is banned under the toolchain baseline. Literal unions and `as const` objects are plain values, are fully type-checked, and tree-shake cleanly.

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
