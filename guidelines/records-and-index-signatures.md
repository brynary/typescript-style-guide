# Records and index signatures

## Rule

Use `Record<K, V>` for a fixed or finite set of known keys, an index signature for an open shape you also give named properties, and `Map` when keys are dynamic, non-string, or the collection changes over its lifetime.

## Why

`Record` and index signatures describe object shapes; `Map` is a real runtime collection with size, iteration, and safe key operations. Under `noUncheckedIndexedAccess`, indexed reads return `V | undefined`, so unknown-key access must be narrowed before use.

## Do

- Use `Record<Currency, number>` when the key set is a known union.
- Use `Map<string, Session>` for caches, registries, and dynamic key sets.
- Narrow an indexed read before using it, since its type includes `undefined`.
- Use a guarded `get` before writing (`Map.getOrInsert` once the default TS lib types it; see [dates-and-stdlib](dates-and-stdlib.md)).

## Example

```ts
type Currency = "usd" | "eur";

const rates: Record<Currency, number> = { usd: 1, eur: 0.9 };

function rateFor(currency: Currency): number {
  return rates[currency];
}

const sessions = new Map<string, number>();

function touch(id: string): number {
  const seen = sessions.get(id);
  const next = seen === undefined ? 1 : seen + 1;
  sessions.set(id, next);
  return next;
}
```
