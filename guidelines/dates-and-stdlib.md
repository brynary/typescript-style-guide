# Dates and Modern Stdlib

## Rule

Use `Temporal`, imported from `temporal-polyfill`, for new date and time code, and reach for modern built-ins (`Object.groupBy`, `RegExp.escape`) before adding a utility library or a legacy idiom.

## Why

`Temporal` gives immutable, unambiguous date/time types that `Date` never provided. Bun does not yet implement `Temporal` and the default TypeScript `lib` does not type it, so the explicit polyfill import supplies both the runtime and the types without touching the compiler baseline. Newer stdlib methods replace whole categories of helper code, so pulling in a dependency for them adds risk without value.

## Do

- Model instants, dates, and durations with `Temporal`; keep values immutable.
- Import it explicitly with `import { Temporal } from "temporal-polyfill"`; drop the import once Bun ships `Temporal` natively.
- Group with `Object.groupBy` and escape user input with `RegExp.escape`.
- Convert a boundary `Date` into `Temporal` immediately on the way in.

## Avoid

- `Date` arithmetic or mutation in new code.
- Referring to `Temporal` as a global or through ambient declarations; Bun does not provide it yet.
- A utility dependency (or a hand-rolled loop) for what a built-in already does.

## Example

```ts
import { Temporal } from "temporal-polyfill";

interface Task {
  readonly id: string;
  readonly status: "open" | "done";
}

export function nextRun(from: Temporal.ZonedDateTime): Temporal.ZonedDateTime {
  return from.add({ hours: 24 });
}

export function toInstant(date: Date): Temporal.Instant {
  return Temporal.Instant.fromEpochMilliseconds(date.getTime());
}

export function byStatus(
  tasks: readonly Task[],
): Partial<Record<Task["status"], readonly Task[]>> {
  return Object.groupBy(tasks, (task) => task.status);
}

export function bump(counts: Map<string, number>, key: string): void {
  const seen = counts.get(key) ?? 0;
  counts.set(key, seen + 1);
}

export function matches(text: string, term: string): boolean {
  return new RegExp(RegExp.escape(term)).test(text);
}
```

## Exceptions

`Date` is allowed at a library or API boundary that requires it; convert to `Temporal` on the near side. For `Promise.try` and other async built-ins, see [async-patterns.md](async-patterns.md). `Map.getOrInsert` is implemented by Bun but not yet typed by the default TypeScript `lib`; use the explicit `get`-check-`set` idiom (as in `bump` above) until the lib includes it, instead of adding ambient declarations.
