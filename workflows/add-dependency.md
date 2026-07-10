# Add a Dependency

Run this workflow to vet, pin, and install a third-party dependency with Bun.

## Required Guidelines

- [bun runtime and dependencies](../guidelines/bun-runtime-and-dependencies.md)
- [type packages and globals](../guidelines/type-packages.md)

Load [monorepo and workspaces](../guidelines/monorepo.md) only when the target is a workspace.

## Workflow

1. Confirm the dependency is actually needed: a Bun API or a modern built-in may
   already cover it ([bun runtime and dependencies](../guidelines/bun-runtime-and-dependencies.md),
   [dates and modern stdlib](../guidelines/dates-and-stdlib.md)). If so, stop.

2. Pick the exact version you intend to pin and check it was published at least
   24 hours ago. Registry inspection is read-only and does not install anything:

   ```sh
   bun info <pkg> time --json
   ```

   Compare the timestamp for your target version to now. If it is younger than
   24 hours, do not install; wait or choose an older version.

3. Add the package with an exact pin (no caret or tilde range):

   ```sh
   bun add --exact <pkg>@<version>
   ```

4. Bun blocks lifecycle scripts on install by default. List anything that was
   blocked and inspect what each script does before trusting it:

   ```sh
   bun pm untrusted
   ```

5. Only if a blocked script is necessary and safe, add the package to
   `trustedDependencies` in `package.json`, then run the scripts:

   ```sh
   bun pm trust <pkg>
   ```

   If the script is not needed, leave the package untrusted.

6. In a monorepo, share the version instead of duplicating it: add the resolved
   version to the root `catalog` and reference it as `"catalog:"` in each package
   that uses it ([monorepo](../guidelines/monorepo.md)):

   ```sh
   bun pm pkg set catalog.<pkg>=<version>
   ```

7. If the package contributes ambient globals (for example a runtime typings
   package), name it in the relevant `tsconfig.json` `types` array; never re-open
   wildcard `@types` discovery ([type packages](../guidelines/type-packages.md)).

8. Verify the workspace still type-checks and passes, then commit `bun.lock`:

   ```sh
   bun install
   bun run typecheck
   bun run lint
   bun test
   ```
