# Barrel Files and Public API Surface

## Rule

Do not create barrel files; curate a package's public surface with the `package.json` `exports` map and keep the module graph free of import cycles.

## Why

Barrel `index.ts` re-export files defeat tree-shaking, slow resolution, and are a common source of import cycles. The `exports` map states the public surface directly, and importing modules by their real path keeps dependencies acyclic and easy to trace.

## Do

- Import each symbol from the module that actually defines it.
- Declare the consumable entry points in `package.json` `exports`.
- Keep internal modules private by leaving them out of `exports`.

## Example

```jsonc
// package.json
{
  "exports": {
    ".": "./src/api.ts",
    "./billing": "./src/billing/api.ts"
  }
}
```

```ts
// consumer imports from the defining module, not a barrel
import { chargeCard } from "./billing/api.ts";
import { formatMoney } from "./billing/money.ts";

export function payAndReceipt(cents: number): string {
  chargeCard(cents);
  return formatMoney({ amount: cents, currency: "USD" });
}
```
