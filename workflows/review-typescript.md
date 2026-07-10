# Review TypeScript Changes

Use this workflow to review a TypeScript diff without loading or restating unrelated policy.

## Guideline Routing

No policy page is universal. Load pages only when the diff raises their concern:

| Changed surface | Load |
| --- | --- |
| Compiler, formatter, or dependencies | [tsconfig](../guidelines/tsconfig-baseline.md), [Biome](../guidelines/biome-baseline.md), [type packages](../guidelines/type-packages.md), [Bun dependencies](../guidelines/bun-runtime-and-dependencies.md) |
| Modules, imports, or exports | [exports](../guidelines/export-style.md), [import paths](../guidelines/import-paths.md), [barrels](../guidelines/barrel-files.md) |
| Domain and boundary types | [null and undefined](../guidelines/null-and-undefined.md), [discriminated unions](../guidelines/discriminated-unions.md), [generics](../guidelines/generics.md), [runtime validation](../guidelines/runtime-validation.md) |
| Functions or classes | [function signatures](../guidelines/function-signatures.md), [classes](../guidelines/classes.md) |
| Errors, async, or cleanup | [error handling](../guidelines/error-handling.md), [async patterns](../guidelines/async-patterns.md), [resource management](../guidelines/resource-management.md) |
| Tests | [test structure](../guidelines/test-structure.md), [test doubles](../guidelines/test-doubles.md) |
| Comments or suppressions | [comments and JSDoc](../guidelines/comments-and-jsdoc.md), [suppressions](../guidelines/suppressions.md) |
| React components or TSX | [React components](../guidelines/react-components.md) |
| Hooks, effects, or context | [React hooks](../guidelines/react-hooks-and-context.md) |
| Executable scripts or automation | [CLI scripts](../guidelines/cli-scripts.md) |
| Workspace or package topology | [monorepo](../guidelines/monorepo.md) |

After loading an overlay, apply it only when its `Activation` section matches.

## Workflow

1. List changed source and configuration files:

   ```sh
   git diff --name-only --diff-filter=d main... -- \
     '*.ts' '*.tsx' 'package.json' 'bunfig.toml' 'tsconfig*.json' 'biome.json*'
   ```

2. Run the repository's mechanical gates first:

   ```sh
   bun run typecheck
   bun run lint
   bun test
   ```

3. Report gate failures directly. Do not manually re-review a mechanical rule after its configured gate passes.
4. Classify the changed surfaces and load only their routed owner pages.
5. Review judgment calls: boundary shape, invalid states, failure handling, cancellation, resource cleanup, test behavior, and justified suppressions.
6. Report only actionable findings. Cite the changed file and line, explain the concrete risk, and link the owner page. State explicitly when no findings remain.

## Avoid

- Do not load unrelated policy pages.
- Do not create a review file unless the user or repository requests one.
- Do not weaken a configured gate merely because it currently fails.
