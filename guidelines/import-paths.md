# Import Paths and Extensions

## Rule

Import nearby files with relative paths and other areas through `package.json` `#` subpath imports, always writing the local `.ts` extension, and never configure `tsconfig` `paths`.

## Why

Relative paths are portable and need no resolver config; `#` subpath imports give stable cross-area names that Node, Bun, and TypeScript all resolve natively. `tsconfig` `paths` is ignored at runtime and is a known Bun monorepo trap. Explicit `.ts` extensions match how Bun runs source directly.

## Do

- Use relative imports (`./`, `../`) within the same area, always ending in `.ts` (or `.tsx`).
- Declare cross-area entry points as `#`-prefixed subpaths in `package.json` `imports` and import them by that name.
- Write wildcard mapping targets without an extension (`"#services/*": "./src/services/*"`); the wildcard captures the `.ts` extension from the import specifier, so a `*.ts` target would resolve to `user.ts.ts`.
- Load JSON and other assets with an import attribute: `with { type: "json" }`.

## Avoid

- `tsconfig` `paths` aliases such as `@/services/*`.
- Extensionless local imports.
- Deep relative chains like `../../../services/user.ts` when a `#` subpath fits.

## Example

```jsonc
// package.json
{
  "imports": {
    // No extension on the target: the specifier's ".ts" fills the wildcard.
    "#services/*": "./src/services/*"
  }
}
```

```ts
import { renderRow } from "./row.ts";
import { loadUser } from "#services/user.ts";
import config from "./config.json" with { type: "json" };

export function greet(id: string): string {
  return renderRow(loadUser(id), config);
}
```

## Exceptions

A `nodenext` island published to Node consumers follows Node's own extension rules; see [module-resolution.md](module-resolution.md). Wildcard asset imports are covered in [side-effect-imports.md](side-effect-imports.md).
