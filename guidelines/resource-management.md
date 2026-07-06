# Resource Management

## Rule

Manage files, connections, and locks with `using` or `await using` on a value that implements `Symbol.dispose` or `Symbol.asyncDispose`.

## Why

A `using` declaration disposes the resource when the scope exits, on every path including thrown errors, without a hand-written `finally`. This makes leaks structurally hard.

## Do

- Give a resource type a `[Symbol.dispose]()` (sync) or `[Symbol.asyncDispose]()` (async) method and implement `Disposable` / `AsyncDisposable`.
- Acquire with `using resource = ...` for sync cleanup, `await using resource = ...` for async cleanup.
- Keep the disposal logic in the resource, not at each call site.

## Avoid

- Manual `close()` / `release()` calls that a `using` binding would run for you.
- Cleanup logic that only runs on the happy path.

## Example

```ts
interface Connection extends AsyncDisposable {
  query(sql: string): Promise<void>;
}

interface Pool {
  acquire(): Promise<Connection>;
}

export function createTimer(label: string): Disposable {
  const start = performance.now();
  return {
    [Symbol.dispose](): void {
      console.log(`${label}: ${performance.now() - start}ms`);
    },
  };
}

export async function runQuery(pool: Pool, sql: string): Promise<void> {
  using timer = createTimer(sql);
  await using connection = await pool.acquire();
  await connection.query(sql);
}
```

## Exceptions

`try`/`finally` is permitted when a resource predates the disposal protocol or when cleanup does not map to a single scoped binding.
