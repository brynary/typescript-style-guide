# Null and undefined

## Rule

Model absence with `undefined`, prefer optional properties over explicit `| undefined` members, and use `null` only at external boundaries, converting it to `undefined` as data enters your code.

## Why

One absence value avoids ambiguous `null`-vs-`undefined` checks. With `exactOptionalPropertyTypes` on, an optional property means "may be absent" and rejects an explicit `undefined` assignment, so `foo?: T` and `foo: T | undefined` mean different things; pick optional for genuine absence.

## Do

- Use optional properties (`foo?: T`) for fields that may be absent.
- Return `T | undefined` from functions that may produce nothing.
- Convert incoming `null` to `undefined` at the boundary (`value ?? undefined`).
- Test absence with `value === undefined` or `value == null` at boundaries only.

## Avoid

- `foo: T | undefined` for a property that is simply optional.
- Introducing `null` in internal data structures or return types.
- Assigning `undefined` to an optional property to mean "present but empty".

## Example

```ts
interface Profile {
  readonly id: string;
  readonly nickname?: string;
}

interface ApiProfile {
  readonly id: string;
  readonly nickname: string | null;
}

function fromApi(raw: ApiProfile): Profile {
  const nickname = raw.nickname ?? undefined;
  return nickname === undefined ? { id: raw.id } : { id: raw.id, nickname };
}
```

## Exceptions

Keep `null` where an external contract requires it: database columns, JSON APIs, and DOM APIs that return `null`. Convert on entry and, where you must emit it, on exit.
