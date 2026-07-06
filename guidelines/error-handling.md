# Error Handling

## Rule

Return a neverthrow `Result` (or `ResultAsync`) for expected, recoverable failures, and `throw` only for bugs and unrecoverable states.

## Why

Expected failures (validation, I/O, domain rules) belong in the type signature so callers must handle them. Reserving `throw` for programmer errors keeps `catch` rare and stack traces meaningful.

## Do

- Model the `E` side as a discriminated error union (`type` field; see [discriminated-unions.md](discriminated-unions.md)) or a typed error class.
- Build values with `ok` / `err`; compose with `andThen`, `map`, and `mapErr`.
- Wrap throwing or async APIs at the boundary with `ResultAsync.fromPromise` or `fromThrowable`.
- Narrow with `catch (error: unknown)` and `instanceof` before touching the value.
- Use `throw` for invariants that indicate a bug (unreachable branch, broken precondition).
- For validation failures feeding the `E` side, see [runtime-validation.md](runtime-validation.md).

## Avoid

- `throw` for outcomes a caller can reasonably recover from.
- `catch (error)` without an explicit `unknown` annotation and narrowing.
- Swallowing a `Result` by ignoring the `err` branch.

## Example

```ts
import { ok, err, Result, ResultAsync } from "neverthrow";

interface Account {
  readonly id: string;
  readonly balance: number;
}

type WithdrawError =
  | { readonly type: "insufficient-funds"; readonly shortfall: number }
  | { readonly type: "unavailable"; readonly reason: string };

export function withdraw(
  account: Account,
  amount: number,
): Result<Account, WithdrawError> {
  if (amount > account.balance) {
    return err({ type: "insufficient-funds", shortfall: amount - account.balance });
  }
  return ok({ id: account.id, balance: account.balance - amount });
}

export function settle(
  load: () => Promise<Account>,
  amount: number,
): ResultAsync<Account, WithdrawError> {
  return ResultAsync.fromPromise(
    load(),
    (error: unknown): WithdrawError => ({
      type: "unavailable",
      reason: error instanceof Error ? error.message : String(error),
    }),
  ).andThen((account) => withdraw(account, amount));
}
```

## Exceptions

Startup misconfiguration and other unrecoverable states may `throw` to halt the process; a caught bug at a top-level boundary may be logged and rethrown.
