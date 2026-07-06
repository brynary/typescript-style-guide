# Async Patterns

## Rule

Use `async`/`await` by default, never leave a promise floating, and run independent work concurrently with `Promise.all` or `Promise.allSettled`.

## Why

`await` keeps control flow linear and readable. An unawaited promise loses errors and ordering, and serial `await`s waste time when the calls do not depend on each other.

## Do

- `await` every promise, or explicitly discard it with `void` for deliberate fire-and-forget.
- Use `Promise.all` when all results are required, `Promise.allSettled` when partial failure is acceptable.
- Thread an `AbortSignal` through cancellable calls and pass it to `fetch`.
- Wrap a value that may be sync or async with `Promise.try`.
- Propagate failures through the error model in [error-handling.md](error-handling.md).

## Avoid

- `.then()` / `.catch()` chains where `await` reads better.
- A bare promise-returning call as a statement (a floating promise).
- Awaiting independent calls one at a time in sequence.

## Example

```ts
interface Profile {
  readonly id: string;
  readonly name: string;
}

export async function fetchAll(
  urls: readonly string[],
  signal: AbortSignal,
): Promise<readonly Response[]> {
  return Promise.all(urls.map((url) => fetch(url, { signal })));
}

export async function settleAll(
  loaders: readonly (() => Promise<Profile>)[],
): Promise<readonly PromiseSettledResult<Profile>[]> {
  return Promise.allSettled(loaders.map((load) => load()));
}

export function normalize(
  make: () => Profile | Promise<Profile>,
): Promise<Profile> {
  return Promise.try(make);
}
```

## Exceptions

Top-level module code and short scripts may use a self-invoking `async` function; a genuinely detached task may be started with `void` once its own errors are handled internally.
