# Type Imports and Import Syntax

## Rule

Import types with a top-level `import type { X } from "..."` statement, kept separate from value imports, and import Node builtins through the `node:` protocol.

## Why

`verbatimModuleSyntax` requires type-only imports to be marked so they erase cleanly and never trigger a runtime module resolution. A top-level `import type` reads at a glance and pairs with Biome's `useImportType`; the `node:` protocol makes builtins unambiguous.

## Do

- Use `import type { Foo } from "./foo.ts"` for anything used only as a type.
- Keep value imports and type imports in separate statements.
- Prefix every Node builtin with `node:`, e.g. `import { readFile } from "node:fs/promises"`.

## Avoid

- Inline `import { type Foo }` mixed with value imports.
- A plain `import { Foo }` when `Foo` is only ever used as a type.
- Bare builtin specifiers like `import { readFile } from "fs/promises"`.

## Example

```ts
import { hostname } from "node:os";
import type { UserRecord } from "./user-record.ts";

export function describeUser(user: UserRecord): string {
  return `${user.name} on ${hostname()}`;
}
```

## Exceptions

None. Import ordering and grouping are applied automatically; see [biome-baseline.md](biome-baseline.md).
