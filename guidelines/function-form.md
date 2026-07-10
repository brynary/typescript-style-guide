# Function Form

## Rule

Write top-level and named functions as `function` declarations, and use arrow functions for callbacks and inline expressions.

## Why

Declarations hoist, read clearly in stack traces, and are easy to grep by name. Arrows are compact for callbacks and capture `this` lexically, which is what inline code almost always wants.

## Do

- Rely on an arrow's lexical `this` inside methods when you need to preserve the enclosing `this`.
- Keep each function to a single form; do not mix a declaration body with a re-exported arrow alias.

## Avoid

- Assigning a named function to a `const` arrow at module top level (`const parse = () => ...`).
- Arrow functions that reference `this` where a plain callback would do.
- `function` expressions assigned to variables when a declaration reads better.
- Relying on `this` inside a standalone `function` callback; bind the value as a parameter instead.

## Example

```ts
function parseTags(raw: string): string[] {
  return raw
    .split(",")
    .map((tag) => tag.trim())
    .filter((tag) => tag.length > 0);
}

class Poller {
  private readonly onTick: () => void;

  constructor(onTick: () => void) {
    this.onTick = onTick;
  }

  start(intervalMs: number): void {
    setInterval(() => this.onTick(), intervalMs);
  }
}
```

## Exceptions

- Use an arrow assigned to a `const` when a framework contract requires a value (for example a hook callback or a memoized handler).
