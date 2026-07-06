# Monorepo and Workspaces

Bun workspace layout, shared versions, and per-package configuration.

## Activation

Load when working in a Bun workspace or adding, linking, or configuring packages in a monorepo. Skip for single-package repos.

## Rule

Link internal packages with the `workspace:*` protocol, share dependency versions through a root catalog, and give each package its own tsconfig and Biome config that extend the root.

## Why

`workspace:*` and catalogs keep every package on one resolved version without caret drift. Per-package tsconfig and Biome files let a package declare exactly its own `types` and rules while inheriting the baseline.

## Do

- Depend on a sibling package with `"@scope/pkg": "workspace:*"`.
- Declare shared third-party versions once in the root `catalog` and reference them with `"catalog:"`.
- Give each package a tsconfig that extends the root and sets its own explicit `types` array (for example `["bun"]` or `["react"]`).
- Add a nested `biome.json` per package that extends the root with `"extends": "//"`.
- Import across packages by their package name; use `#` subpath imports inside a package. See [import-paths.md](import-paths.md).

## Avoid

- `tsconfig` `paths` aliases for internal linking; the ban is owned by [import-paths.md](import-paths.md).
- Caret ranges for a dependency shared by several packages; catalog it instead.
- A single root tsconfig `types` array that leaks globals into every package.

## Example

```jsonc
// root package.json
{
  "workspaces": ["packages/*"],
  "catalog": { "zod": "4.0.5" }
}
```

```jsonc
// packages/api/package.json
{
  "dependencies": {
    "@scope/core": "workspace:*",
    "zod": "catalog:"
  }
}
```

```jsonc
// packages/api/tsconfig.json
{
  "extends": "../../tsconfig.json",
  "compilerOptions": { "types": ["bun"] }
}
```

```ts
// packages/api/src/handler.ts
import { parseUser } from "@scope/core";
import { logger } from "#lib/logger.ts";

export function handle(input: unknown): void {
  logger.info(parseUser(input));
}
```

## Exceptions

A package that ships to Node consumers may use a `nodenext` tsconfig island; see [module-resolution.md](module-resolution.md).
