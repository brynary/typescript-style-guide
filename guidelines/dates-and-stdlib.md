# Dates and Modern Stdlib

## Rule

Use `Temporal` for new date and time code, and reach for modern built-ins (`Map.getOrInsert`, `RegExp.escape`, `Object.groupBy`) before adding a utility library or a legacy idiom.

## Why

`Temporal` gives immutable, unambiguous date/time types that `Date` never provided. Newer stdlib methods replace whole categories of helper code, so pulling in a dependency for them adds risk without value.

## Do

- Model instants, dates, and durations with `Temporal`; keep values immutable.
- Group with `Object.groupBy`, count/cache with `Map.getOrInsert`, and escape user input with `RegExp.escape`.
- Convert a boundary `Date` into `Temporal` immediately on the way in.

## Avoid

- `Date` arithmetic or mutation in new code.
- A utility dependency (or a hand-rolled loop) for what a built-in already does.

## Example

```ts
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
  counts.set(key, counts.getOrInsert(key, 0) + 1);
}

export function matches(text: string, term: string): boolean {
  return new RegExp(RegExp.escape(term)).test(text);
}
```

## Exceptions

`Date` is allowed at a library or API boundary that requires it; convert to `Temporal` on the near side. For `Promise.try` and other async built-ins, see [async-patterns.md](async-patterns.md). If the project's TypeScript `lib` does not yet type `Map.getOrInsert`, use the explicit `get`-check-`set` idiom instead of adding ambient declarations.
