# Type-Check Workflow

## Rule

Run one root `typecheck` script in a pre-commit hook and CI: use `tsc --noEmit` in a single package, and have a workspace root orchestrate that script in every package.

## Why

Bun and Vite erase types without checking them. The root script is the authoritative gate even when it delegates to package tsconfigs.

## Do

- Give each checked package a `typecheck` script that runs `tsc --noEmit` against its extending tsconfig.
- In a single package, expose that script at the root.
- In a workspace, expose a root script that runs every package's script (see [monorepo](monorepo.md)).
- Run the root script in a pre-commit or pre-push hook and as a required CI check.

## Avoid

- Relying on editor diagnostics or `bun run` alone to catch type errors.
- Assuming a green `bun test` or a successful `vite build` means the code type-checks.
- Checking only a workspace's root baseline while skipping package tsconfigs.
- Adding a looser tsconfig just for the gate.

## Example

```jsonc
// Single-package package.json
{
  "scripts": {
    "typecheck": "tsc --noEmit"
  }
}
```

```jsonc
// Workspace root package.json; each package defines "typecheck": "tsc --noEmit"
{
  "scripts": {
    "typecheck": "bun run --workspaces typecheck"
  }
}
```

Both hooks and CI run `bun run typecheck` from the repository root.
