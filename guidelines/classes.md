# Classes

## Rule

Reach for functions first, and use a class only for a stateful abstraction or a framework contract that requires one.

## Why

Most logic is clearer as functions over plain data. A class earns its place when instances own mutable state, a lifecycle, or an interface a framework instantiates.

## Do

- Mark hidden members with the TypeScript `private` keyword.
- Declare each field explicitly and assign it in the constructor body.
- Mark every method that replaces a base method with `override` (enforced by `noImplicitOverride`).
- Keep public methods with an explicit return type, per [function-signatures.md](function-signatures.md).

## Avoid

- `#private` fields; use the `private` keyword instead.
- Constructor parameter properties (`constructor(private x: number)`); they are banned by the erasable-syntax baseline in [tsconfig-baseline.md](tsconfig-baseline.md).
- Overriding a base method without the `override` keyword.

## Example

```ts
class TokenBucket {
  private readonly capacity: number;
  private tokens: number;

  constructor(capacity: number) {
    this.capacity = capacity;
    this.tokens = capacity;
  }

  tryTake(): boolean {
    if (this.tokens <= 0) {
      return false;
    }
    this.tokens -= 1;
    return true;
  }

  refill(): void {
    this.tokens = this.capacity;
  }
}

class BurstBucket extends TokenBucket {
  override refill(): void {
    super.refill();
    super.refill();
  }
}
```
