# Readonly and immutability

## Rule

Default to `readonly` for the properties of DTOs, component props, and public return types, use `readonly T[]` for array fields, and produce new values by copy-and-return instead of mutating inputs in place.

## Why

Immutable-by-default types prevent accidental mutation of shared data and make data flow easy to follow. `readonly` is a compile-time guarantee with no runtime cost.

## Do

- Mark shape properties `readonly` and array fields `readonly T[]`.
- Wrap an existing shape with `Readonly<T>` when you cannot annotate each field.
- Return a new object or array (spread, `map`, `filter`) rather than mutating a parameter.
- Keep `readonly` on parameters that the function must not mutate.

## Avoid

- Mutating an argument and returning it.
- Exposing a mutable array or object from a public return type.
- `push`, `splice`, or property assignment on shared state.

## Example

```ts
interface Cart {
  readonly items: readonly CartItem[];
  readonly total: number;
}

interface CartItem {
  readonly sku: string;
  readonly price: number;
}

function addItem(cart: Cart, item: CartItem): Cart {
  const items = [...cart.items, item];
  return { items, total: cart.total + item.price };
}
```

## Exceptions

Local variables inside a function may be mutated freely; the rule governs shared data, inputs, and public surfaces. Performance-critical hot paths may mutate a locally constructed value before returning it, as long as the mutation never escapes.
