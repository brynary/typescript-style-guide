# Resource Management

## Rule

Use `using` or `await using` for a resource that implements the disposal protocol; otherwise guarantee cleanup with `try`/`finally`.

## Why

A `using` declaration disposes the resource when the scope exits, on every path including thrown errors, without a hand-written `finally`. This makes leaks structurally hard.

## Do

- Give a resource type a `[Symbol.dispose]()` (sync) or `[Symbol.asyncDispose]()` (async) method and implement `Disposable` / `AsyncDisposable`.
- Acquire with `using resource = ...` for sync cleanup, `await using resource = ...` for async cleanup.
- Prefix a disposal-only binding with an underscore (`using _timer = ...`); `noUnusedLocals` exempts underscore-prefixed names, and the binding exists only for its scope-exit effect.
- Keep the disposal logic in the resource, not at each call site.
- Use `try`/`finally` when an existing API does not expose one scoped disposable value.

## Avoid

- Manual `close()` / `release()` calls that a `using` binding would run for you.
- Cleanup logic that only runs on the happy path.

## Example

```ts
import { ResultAsync } from "neverthrow";

interface Connection extends AsyncDisposable {
  query(sql: string): Promise<void>;
}

interface Pool {
  acquire(): Promise<Connection>;
}

interface QueryError {
  readonly type: "query-failed";
  readonly reason: string;
}

export function createTimer(label: string): Disposable {
  const start = performance.now();
  return {
    [Symbol.dispose](): void {
      console.log(`${label}: ${performance.now() - start}ms`);
    },
  };
}

export function runQuery(pool: Pool, sql: string): ResultAsync<void, QueryError> {
  return ResultAsync.fromPromise(
    (async () => {
      using _timer = createTimer(sql);
      await using connection = await pool.acquire();
      await connection.query(sql);
    })(),
    (error: unknown): QueryError => ({
      type: "query-failed",
      reason: error instanceof Error ? error.message : String(error),
    }),
  );
}
```
