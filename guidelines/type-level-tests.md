# Type-Level Tests

## Rule

Write `expectTypeOf` type-level tests only for the exported public types of a library package, and not for application code.

## Why

A library's exported types are an API contract that inference can silently break; type tests lock that contract so a refactor that changes an inferred shape fails loudly. Application code has no external type consumers, so type tests there only duplicate what `tsc` already checks.

## Do

- Add type-level tests for exported generics, discriminated unions, overload results, and other inference-sensitive public types of a library.
- Use `expectTypeOf` from `bun:test` inside a normal `test`, colocated in the package's `*.test.ts` files.
- Assert the resolved type, not just assignability, when the exact shape is the contract.

## Avoid

- Type tests for application (non-library) code.
- Type tests for internal, non-exported types.
- `as`-based casts to force a type test to pass; assert the real inferred type.

## Example

```ts
import { describe, test, expectTypeOf } from "bun:test";
import { parse } from "./result.ts";
import type { Parsed } from "./result.ts";

describe("parse public type", () => {
  test("narrows to the success branch", () => {
    const value = parse("42");
    expectTypeOf(value).toEqualTypeOf<Parsed>();
  });

  test("exposes a readonly value field", () => {
    expectTypeOf<Parsed>().toHaveProperty("value").toBeNumber();
  });
});
```

## Exceptions

None. Runtime test structure is owned by [test-structure.md](test-structure.md).
