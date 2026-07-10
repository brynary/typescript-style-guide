# Review TypeScript Changes

Use this workflow to review a TypeScript diff against the guide without loading or restating unrelated policy.

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

   Report gate failures directly. Do not re-review a mechanical rule by hand after its configured gate passes.

3. Use [guidelines.md](../guidelines.md) as the router. Load an owner page only when the diff raises its concern, then review the judgment calls the gates cannot settle:

   - If configuration or dependencies changed, compare them with [tsconfig baseline](../guidelines/tsconfig-baseline.md), [Biome baseline](../guidelines/biome-baseline.md), [type packages](../guidelines/type-packages.md), or [Bun runtime and dependencies](../guidelines/bun-runtime-and-dependencies.md). A weakened gate can still pass.
   - Is the module boundary intentional and minimal? Check [export style](../guidelines/export-style.md), [import paths](../guidelines/import-paths.md), and [barrel files](../guidelines/barrel-files.md) when imports or exports change.
   - Do types make invalid states unrepresentable without needless complexity? Check the relevant type-modeling page, especially [null and undefined](../guidelines/null-and-undefined.md), [discriminated unions](../guidelines/discriminated-unions.md), or [generics](../guidelines/generics.md).
   - Do public functions have focused signatures, and are classes justified by state or a framework contract? Check [function signatures](../guidelines/function-signatures.md) and [classes](../guidelines/classes.md).
   - Are recoverable failures represented by `Result`, untrusted inputs validated once, and cancellation or cleanup explicit? Check [error handling](../guidelines/error-handling.md), [runtime validation](../guidelines/runtime-validation.md), [async patterns](../guidelines/async-patterns.md), and [resource management](../guidelines/resource-management.md).
   - Do tests cover behavior through real implementations or narrow boundary doubles? Check [test structure](../guidelines/test-structure.md) and [test doubles](../guidelines/test-doubles.md).
   - Do comments explain only non-obvious reasons or contracts? Check [comments and JSDoc](../guidelines/comments-and-jsdoc.md).
   - Is every new suppression narrow, justified, and isolated? Check [suppressions](../guidelines/suppressions.md).

4. Load an overlay only when its activation condition matches the diff: [React components](../guidelines/react-components.md), [React hooks and context](../guidelines/react-hooks-and-context.md), [CLI scripts](../guidelines/cli-scripts.md), or [monorepo](../guidelines/monorepo.md).

5. Report only actionable findings. For each finding, cite the changed file and line, explain the concrete risk, and link the owner page. State explicitly when no findings remain. Use the output location the user or repository requests; do not create a review file by default.
