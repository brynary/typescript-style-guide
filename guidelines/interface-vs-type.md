# Interface vs Type

## Rule

Use `interface` to declare object shapes and `type` for everything else (unions, intersections, tuples, mapped types, and aliases of primitives or functions).

## Why

One consistent choice removes a per-declaration decision. `interface` gives better error messages for object shapes and supports declaration merging when augmenting third-party types.

## Do

- Declare object and class-implementable shapes with `interface`.
- Use `type` for unions, tuples, function types, and aliases (`type Id = string`).
- Extend object shapes with `interface B extends A`.

## Avoid

- Aliasing an object shape with `type` when `interface` fits.
- Mixing both styles for object shapes in the same package.

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

## Exceptions

None. This is enforced mechanically by Biome `useConsistentTypeDefinitions`; see [biome-baseline](biome-baseline.md).
