# Type-Check Workflow

## Rule

Type checking is a separate step: run `tsc --noEmit` against the root `tsconfig.json` in a pre-commit hook and in CI, because Bun and Vite strip types without checking them.

## Why

Bun's loader and Vite transpile by erasing type annotations for speed; neither reports type errors. Nothing verifies types unless `tsc` runs explicitly, so an unchecked repo can ship code that never type-checked.

## Do

- Expose a `typecheck` script that runs `tsc --noEmit` and treat it as the authoritative gate.
- Run it in a pre-commit (or pre-push) hook so errors are caught before code leaves a machine.
- Run the same script in CI as a required check.
- Treat the root `tsconfig.json` (see [tsconfig-baseline](tsconfig-baseline.md)) as authoritative; run against it, not an editor-only config.

## Avoid

- Relying on editor diagnostics or `bun run` alone to catch type errors.
- Assuming a green `bun test` or a successful `vite build` means the code type-checks.
- Adding a second, looser tsconfig just for the type-check step.

## Example

```jsonc
{
  "scripts": {
    // Authoritative type-check gate; runs in pre-commit and CI
    "typecheck": "tsc --noEmit"
  }
}
```

Wire the same script into both gates: a pre-commit hook runs `bun run typecheck`, and the CI workflow runs `bun run typecheck` as a required job.

## Exceptions

- None. Type checking always runs against the root config in both gates.
