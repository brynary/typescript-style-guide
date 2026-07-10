# Type Imports and Import Syntax

## Rule

Import types with a top-level `import type { X } from "..."` statement, kept separate from value imports, and import Node builtins through the `node:` protocol.

## Why

`verbatimModuleSyntax` requires type-only imports to be marked so they erase cleanly and never trigger a runtime module resolution. A top-level `import type` reads at a glance and pairs with Biome's `useImportType`; the `node:` protocol makes builtins unambiguous.

## Example

```ts
import { hostname } from "node:os";
import type { UserRecord } from "./user-record.ts";

export function describeUser(user: UserRecord): string {
  return `${user.name} on ${hostname()}`;
}
```
