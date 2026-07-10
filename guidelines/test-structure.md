# Test Structure and Runner

## Rule

Write tests with `bun:test` using `test` and `describe` (never `it`), keep each test self-contained with its own fixtures, and never use loops or shared mutable state in a test.

## Why

`test` + `describe` matches the bun:test docs and reads uniformly across the codebase. Self-contained tests with fresh fixtures make failures local and order-independent; loops and shared mutable state hide which case failed and let one test corrupt another.

## Do

- Import `describe`, `test`, `expect`, and lifecycle hooks from `bun:test`.
- Group related cases in a `describe` and name each `test` for the behavior it checks.
- Build fresh fixtures inside each `test`; extract a helper function when several tests need the same starting state.
- Reserve `beforeEach`/`afterEach` for side-effectful setup and cleanup (spies, temp files), not for assigning shared fixture bindings.
- Write one `test` per case; assert the specific case directly.

## Avoid

- `it(...)` and BDD `should` phrasing.
- Loops (`for`, `forEach`, `test.each`) that fold several cases into one `test`.
- Module- or describe-scoped `let` fixtures reassigned in `beforeEach`, or reused arrays and objects shared between tests.

## Example

```ts
import { describe, expect, test } from "bun:test";
import { addItem, createCart } from "./cart.ts";

describe("cart", () => {
  test("starts empty", () => {
    const cart = createCart();
    expect(cart.items).toHaveLength(0);
  });

  test("records an added item", () => {
    const cart = createCart();
    const updated = addItem(cart, { sku: "a1", price: 500 });
    expect(updated.items).toEqual([{ sku: "a1", price: 500 }]);
  });
});
```
