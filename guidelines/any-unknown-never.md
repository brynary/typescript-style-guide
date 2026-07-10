# any, unknown, and never

## Rule

Never use `any` in production code; type untrusted values as `unknown` and narrow them, use `never` for impossibility, and never use the boxed spellings `Object`, `String`, `Number`, `Boolean`, `Function`, or bare `{}`.

## Why

`any` disables checking and spreads silently through inference. `unknown` forces a narrowing step at the trust boundary, so external data cannot flow into typed code unchecked. The boxed globals and `{}` accept almost any value and defeat the point of a type.

## Do

- Accept external input as `unknown` and narrow with a type guard or schema.
- Use `never` for unreachable branches and impossible states.
- Use `object` for "any non-primitive" and `Record<string, unknown>` for open maps.
- Type a callback as `(...args: readonly unknown[]) => unknown`, not `Function`.
- Validate external data with Zod where it is in use (see [runtime-validation](runtime-validation.md)).
- Isolate untyped third-party interop behind a wrapper that exposes `unknown` and carries a justified suppression (see [suppressions](suppressions.md)).

## Example

```ts
interface Config {
  readonly retries: number;
}

function isConfig(value: unknown): value is Config {
  return (
    typeof value === "object" &&
    value !== null &&
    "retries" in value &&
    typeof value.retries === "number"
  );
}

```
