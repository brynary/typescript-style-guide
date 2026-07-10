# Comments and JSDoc

## Rule

Write comments only to explain why non-obvious code exists, add JSDoc only where an exported API's contract is not obvious from its signature, and never narrate what the code does or that you changed it.

## Why

Types and clear names already say what the code does, so what-comments are noise that drifts out of date. A comment earns its place only when the reason for the code cannot be read from the code itself.

## Do

- Comment the reason: a constraint, a workaround, a non-obvious tradeoff, or a link to context.
- Add JSDoc to an exported function or type when its signature does not convey units, ranges, error conditions, or intended use.
- Write TODOs as `// TODO(owner): action` so they are attributable and greppable.

## Avoid

- What-comments that restate the code (`// increment i`, `// return the user`).
- Change narration (`// renamed from foo`, `// added null check`); git history owns that.
- Commented-out code; delete it.
- JSDoc that repeats the type signature with no added contract.

## Example

```ts
import { ResultAsync } from "neverthrow";

interface RetryOptions {
  readonly attempts: number;
}

interface RetryError {
  readonly type: "retry-exhausted";
  readonly cause: unknown;
}

/**
 * Retries `run` with exponential backoff.
 * Returns a retry-exhausted error with the final cause after `attempts` failures.
 */
export function withRetry<T>(
  run: () => Promise<T>,
  options: RetryOptions,
): ResultAsync<T, RetryError> {
  // The vendor API rejects bursts under 200ms apart, so never retry faster.
  const minDelayMs = 200;
  return ResultAsync.fromPromise(
    attempt(run, options.attempts, minDelayMs),
    (cause: unknown): RetryError => ({ type: "retry-exhausted", cause }),
  );
}
```
