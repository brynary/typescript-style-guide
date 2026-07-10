# Async Patterns

## Rule

Use `async`/`await`, run independent work concurrently, and either await each promise or deliberately detach it with `void` after the task handles its own errors.

## Why

`await` keeps control flow linear. Concurrency avoids unnecessary serial waits, while explicit detachment makes ownership of a background task visible.

## Do

- Use `Promise.all` for independent tasks whose expected failures are represented by `Result`.
- Reserve `Promise.allSettled` for top-level best-effort orchestration that must observe every unexpected rejection.
- Thread an `AbortSignal` through cancellable calls and pass it to `fetch`.
- Normalize a value that may be sync or async with `Promise.try`, then wrap recoverable failure in `ResultAsync`.
- Start a detached task with `void` only after it handles its own expected and unexpected failures.

## Avoid

- `.then()` / `.catch()` chains where `await` reads better.
- A bare promise-returning call as a statement (a floating promise).
- Awaiting independent calls one at a time in sequence.

## Example

```ts
import type { Result } from "neverthrow";
import { ResultAsync } from "neverthrow";

interface Profile {
  readonly id: string;
  readonly name: string;
}

interface LoadError {
  readonly type: "load-failed";
  readonly reason: string;
}

function toLoadError(error: unknown): LoadError {
  return {
    type: "load-failed",
    reason: error instanceof Error ? error.message : String(error),
  };
}

export async function loadAll(
  loaders: readonly ((signal: AbortSignal) => ResultAsync<Profile, LoadError>)[],
  signal: AbortSignal,
): Promise<readonly Result<Profile, LoadError>[]> {
  return Promise.all(loaders.map((load) => load(signal)));
}

export function normalizeLoad(
  make: () => Profile | Promise<Profile>,
): ResultAsync<Profile, LoadError> {
  return ResultAsync.fromPromise(Promise.try(make), toLoadError);
}
```
