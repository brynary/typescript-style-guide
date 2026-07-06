# Bun Runtime and Dependencies

## Rule

Use Bun as runtime, package manager, and script runner; prefer Bun's own APIs over Node-portable equivalents; pin every dependency to an exact version and share versions through workspace catalogs.

## Why

Bun runs TypeScript directly and installs faster with a single committed lockfile, so a Bun-first project needs no separate build or Node shim. Exact pins plus catalogs make installs reproducible and keep shared versions in one place.

## Do

- Manage dependencies with `bun install` / `bun add` and run scripts with `bun run`; commit `bun.lock`.
- Reach for Bun APIs first: `Bun.file`, `Bun.serve`, and the `Bun.$` shell instead of `fs`, `http`, or `child_process`.
- Pin exact versions (`bun add --exact`, or `[install] exact = true` in `bunfig.toml`); no caret or tilde ranges.
- Put shared versions in a workspace catalog and reference them with `catalog:` (see [monorepo](monorepo.md)).
- Never install a package published less than 24 hours ago.
- Review a dependency's postinstall script before allowing it; Bun blocks lifecycle scripts until the package is added to `trustedDependencies`.

## Avoid

- `npm`, `yarn`, or `pnpm` commands and their lockfiles.
- Writing Node-portable code (`node:fs`, `node:http`) when a Bun API covers the case.
- Range specifiers or duplicated version literals across packages.
- Adding a package to `trustedDependencies` without reading what its install script does.

## Example

```ts
export async function readConfig(path: string): Promise<string> {
	return Bun.file(path).text();
}
```

```toml
# bunfig.toml
[install]
exact = true
```

```jsonc
{
  "dependencies": {
    "zod": "4.0.5", // exact pin
    "@app/core": "catalog:" // shared version from the workspace catalog
  },
  "trustedDependencies": ["some-native-addon"]
}
```

## Exceptions

- A package published to Node consumers may keep Node-portable APIs at its public boundary (see [module-resolution](module-resolution.md)).
