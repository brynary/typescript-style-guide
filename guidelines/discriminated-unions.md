# Discriminated unions

## Rule

Model a value that is exactly one of several shapes as a discriminated union with a shared literal field named `type`, and handle it with a `switch` that ends in an `assertNever` exhaustiveness check.

## Why

A discriminated union makes invalid combinations unrepresentable, unlike a bag of optional fields and boolean flags. The `assertNever` default turns a newly added variant into a compile error at every unhandled `switch`.

## Do

- Give every member a `type` field with a distinct string literal.
- Narrow with `switch (value.type)` and let each case see the full member.
- End the `switch` with a `default` that calls `assertNever(value)`.
- Write small guard helpers (`value.type === "ok"`) where a full switch is overkill.

## Avoid

- A single shape with `isError`, `data?`, and `error?` all optional.
- A discriminant field named `kind` or `_tag` for your own unions.
- A `switch` with no exhaustiveness guard.

## Example

```ts
interface Loading {
  readonly type: "loading";
}

interface Loaded {
  readonly type: "loaded";
  readonly value: string;
}

interface Failed {
  readonly type: "failed";
  readonly message: string;
}

type State = Loading | Loaded | Failed;

function assertNever(value: never): never {
  throw new Error(`unexpected variant: ${JSON.stringify(value)}`);
}

function render(state: State): string {
  switch (state.type) {
    case "loading":
      return "...";
    case "loaded":
      return state.value;
    case "failed":
      return state.message;
    default:
      return assertNever(state);
  }
}
```

## Exceptions

Values from a Result library keep that library's own discriminant convention; see [error-handling](error-handling.md). The `type` field name applies to unions you define.
