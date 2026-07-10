# Set Up a New Project

Run this workflow to scaffold a new Bun + TypeScript 6 + Biome project on the guide's baselines.

## Guideline Routing

- [bun runtime and dependencies](../guidelines/bun-runtime-and-dependencies.md)
- [tsconfig baseline](../guidelines/tsconfig-baseline.md)
- [type packages and globals](../guidelines/type-packages.md)
- [module resolution baseline](../guidelines/module-resolution.md)
- [biome baseline](../guidelines/biome-baseline.md)
- [project layout and file naming](../guidelines/project-layout.md)
- [type-check workflow](../guidelines/type-check-workflow.md)

## Workflow

1. Initialize the package and remove the generated sample files:

   ```sh
   mkdir my-app && cd my-app
   bun init -y
   rm -f index.ts
   ```

2. Force exact version pins for every future install by writing `bunfig.toml`:

   ```sh
   printf '[install]\nexact = true\n' > bunfig.toml
   ```

3. Add the toolchain as exact-pinned dev dependencies:

   ```sh
   bun add --dev --exact typescript @biomejs/biome
   ```

4. Set `"type": "module"` in `package.json` and add the gate scripts:

   ```sh
   bun pm pkg set type=module
   bun pm pkg set module=src/main.ts
   bun pm pkg set scripts.typecheck="tsc --noEmit"
   bun pm pkg set scripts.lint="biome check ."
   bun pm pkg set scripts.format="biome format --write ."
   bun pm pkg set scripts.test="bun test"
   ```

5. Create `tsconfig.json` and paste the canonical config verbatim from
   [tsconfig baseline](../guidelines/tsconfig-baseline.md); keep its `types: ["bun"]`
   ([type packages](../guidelines/type-packages.md)) and `module: "preserve"` /
   `moduleResolution: "bundler"` ([module resolution](../guidelines/module-resolution.md)) unchanged.

6. Create `biome.json` and paste the canonical config verbatim from
   [biome baseline](../guidelines/biome-baseline.md); do not override its formatter defaults.

7. Create the layer folders and a `src/main.ts` entry, per
   [project layout](../guidelines/project-layout.md):

   ```sh
   mkdir -p src/routes src/services src/models
   printf 'export function main(): void {}\n' > src/main.ts
   ```

8. Wire the type check and lint into a version-controlled pre-commit hook, per
   [type-check workflow](../guidelines/type-check-workflow.md):

   ```sh
   mkdir -p .githooks
   printf '#!/usr/bin/env bash\nset -euo pipefail\nbun run typecheck\nbun run lint\n' > .githooks/pre-commit
   chmod +x .githooks/pre-commit
   git config core.hooksPath .githooks
   ```

9. Add a CI job that runs the same gate as a required check:

   ```sh
   bun install
   bun run typecheck
   bun run lint
   bun test
   ```

10. Commit the scaffold, including `bun.lock`, `bunfig.toml`, `tsconfig.json`,
    `biome.json`, and `.githooks/`.
