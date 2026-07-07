# Variables and Naming

## Rule

Declare bindings with `const` first (`let` only when reassigned, never `var`), and name symbols with the Google-baseline scheme.

## Why

`const` communicates that a binding never changes, which agents and readers can rely on. One consistent naming scheme keeps identifiers greppable and predictable.

## Do

- Use `const` for every binding that is not reassigned; use `let` only for a value that is genuinely reassigned.
- Name values, functions, methods, and parameters in `lowerCamelCase`.
- Name types, interfaces, classes, and enums-replacements in `UpperCamelCase`.
- Name module-level constants (fixed, primitive, top-level) in `CONSTANT_CASE`.
- Give booleans a predicate name (`isReady`, `hasItems`).
- Case acronyms as words (`HttpClient`, `parseUrl`, `userId`), never as all-caps runs (`HTTPClient`, `parseURL`).
- File naming is owned by [project-layout.md](project-layout.md).

## Avoid

- `var` in any code.
- `let` for a binding you never reassign.
- An `I` prefix on interfaces or a `T` prefix on types.
- Hungarian notation or type suffixes (`userStr`, `countNum`).
- Abbreviations that are not widely accepted (`usrCfg`, `resp`); write the whole word.
- `CONSTANT_CASE` for local bindings or object properties.

## Example

```ts
const MAX_RETRIES = 3;

interface RetryOptions {
  readonly maxRetries: number;
  readonly delayMs: number;
}

function nextDelayMs(attempt: number, options: RetryOptions): number {
  const backoffMs = options.delayMs * 2 ** attempt;
  const isLastAttempt = attempt >= options.maxRetries;
  return isLastAttempt ? 0 : backoffMs;
}
```

## Exceptions

- Keep names dictated by an external contract (framework props, wire formats, environment variables) exactly as the contract spells them.
