# Migrate Legacy Code

Run this workflow to bring an existing TypeScript package up to the guide's baselines.

## Required Guidelines

- [tsconfig baseline](../guidelines/tsconfig-baseline.md)
- [biome baseline](../guidelines/biome-baseline.md)
- [type-check workflow](../guidelines/type-check-workflow.md)
- [enums](../guidelines/enums.md)
- [module resolution baseline](../guidelines/module-resolution.md)
- [classes](../guidelines/classes.md)
- [barrel files](../guidelines/barrel-files.md)
- [any, unknown, never](../guidelines/any-unknown-never.md)
- [assertions and satisfies](../guidelines/assertions-and-satisfies.md)
- [dates and modern stdlib](../guidelines/dates-and-stdlib.md)
- [suppressions and escape hatches](../guidelines/suppressions.md)

## Workflow

1. Adopt the baselines: replace the project `tsconfig.json` with the canonical
   config from [tsconfig baseline](../guidelines/tsconfig-baseline.md) and the
   `biome.json` from [biome baseline](../guidelines/biome-baseline.md). This turns
   the remaining steps into a concrete worklist.

2. Generate the worklist by running both gates and saving the output:

   ```sh
   bun run typecheck 2>&1 | tee migrate-typecheck.txt
   bun run lint 2>&1 | tee migrate-lint.txt
   ```

3. Fix in dependency order, re-running the gates after each step. Start with the
   syntax `erasableSyntaxOnly` now rejects:

   - Remove deprecated TS 6/7 compiler options flagged by `tsc`; keep only the canonical flags ([tsconfig baseline](../guidelines/tsconfig-baseline.md)).
   - Replace every `enum` and `const enum` with an `as const` object or a literal union ([enums](../guidelines/enums.md)).
   - Convert `namespace` blocks into ES modules with named exports ([module resolution](../guidelines/module-resolution.md)).
   - Rewrite constructor parameter properties as explicitly declared fields assigned in the constructor body ([classes](../guidelines/classes.md)).

4. Convert CommonJS to ESM: replace `require`/`module.exports` with `import`/`export`
   and `__dirname`/`__filename` with `import.meta` equivalents ([module resolution](../guidelines/module-resolution.md)).

5. Remove barrel files: delete `index.ts` re-export modules and repoint each
   importer at the module that defines the symbol; break any resulting cycles
   ([barrel files](../guidelines/barrel-files.md)).

6. Eliminate escape hatches: replace `any` with `unknown` plus narrowing, and
   remove `as` and `!` in favor of guards or `satisfies` ([any, unknown, never](../guidelines/any-unknown-never.md),
   [assertions and satisfies](../guidelines/assertions-and-satisfies.md)).

7. Migrate date logic: convert `Date` arithmetic in new and touched code to
   `Temporal`, converting boundary `Date` values on entry ([dates and modern stdlib](../guidelines/dates-and-stdlib.md)).

8. Where a fix is too large to land at once, bridge it with a suppression that
   carries a written reason and a follow-up, never a bare directive
   ([suppressions](../guidelines/suppressions.md)):

   ```sh
   grep -rn "@ts-expect-error\|biome-ignore" src
   ```

9. For a risky change to a public API, use expand-and-contract: add the new shape
   alongside the old one, migrate every caller, then remove the old shape in a
   later change once nothing references it.

10. Re-run the gates until both pass with no remaining suppressions beyond the
    justified bridges, then commit:

    ```sh
    bun run typecheck
    bun run lint
    bun test
    ```

## Avoid

- Relaxing a baseline flag to make legacy code compile instead of fixing it.
- Leaving bridge suppressions without a reason or a follow-up, or letting them become permanent.
- Renaming or removing a public API in place without expand-and-contract.
- Migrating everything in one commit so a failing gate cannot be traced to a step.
