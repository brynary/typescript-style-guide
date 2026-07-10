# Interface vs Type

## Rule

Use `interface` to declare object shapes and `type` for everything else (unions, intersections, tuples, mapped types, and aliases of primitives or functions).

## Why

One consistent choice removes a per-declaration decision. `interface` gives better error messages for object shapes and supports declaration merging when augmenting third-party types.

## Do

- Extend object shapes with `interface B extends A`.

## Example

```ts
interface User {
  readonly id: string;
  readonly name: string;
}

interface Admin extends User {
  readonly permissions: readonly string[];
}

type Id = string;
type Shape = Circle | Square;

interface Circle {
  readonly type: "circle";
  readonly radius: number;
}

interface Square {
  readonly type: "square";
  readonly side: number;
}
```
