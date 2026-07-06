# Test Structure and Runner

## Rule

Write tests with `bun:test` using `test` and `describe` (never `it`), keep each test self-contained with its own fixtures, and never use loops or shared mutable state in a test.

## Why

`test` + `describe` matches the bun:test docs and reads uniformly across the codebase. Self-contained tests with fresh fixtures make failures local and order-independent; loops and shared mutable state hide which case failed and let one test corrupt another.

## Do

- Import `describe`, `test`, `expect`, and lifecycle hooks from `bun:test`.
- Group related cases in a `describe` and name each `test` for the behavior it checks.
- Build fresh fixtures inside each `test`, or in `beforeEach` when several tests need the same starting state.
- Reset any per-test state in `afterEach` so tests do not leak into each other.
- Write one `test` per case; assert the specific case directly.

## Avoid

- `it(...)` and BDD `should` phrasing.
- Loops (`for`, `forEach`, `test.each`) that fold several cases into one `test`.
- Module-level `let` fixtures mutated across tests, or reused arrays and objects shared between tests.

## Example

```ts
import { describe, test, expect, beforeEach } from "bun:test";
import { createCart, addItem } from "./cart.ts";
import type { Cart } from "./cart.ts";

describe("cart", () => {
  let cart: Cart;

  beforeEach(() => {
    cart = createCart();
  });

  test("starts empty", () => {
    expect(cart.items).toHaveLength(0);
  });

  test("records an added item", () => {
    const updated = addItem(cart, { sku: "a1", price: 500 });
    expect(updated.items).toEqual([{ sku: "a1", price: 500 }]);
  });
});
```

## Exceptions

None. Test file naming and placement are owned by [project-layout.md](project-layout.md). For mocks and restoring state see [test-doubles.md](test-doubles.md).
