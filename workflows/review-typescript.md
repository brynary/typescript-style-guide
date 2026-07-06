# Review TypeScript Changes

Run this workflow to review a TypeScript diff against the guide before it merges.

## Required Guidelines

- [tsconfig baseline](../guidelines/tsconfig-baseline.md)
- [biome baseline](../guidelines/biome-baseline.md)
- [type packages and globals](../guidelines/type-packages.md)
- [bun runtime and dependencies](../guidelines/bun-runtime-and-dependencies.md)
- [project layout and file naming](../guidelines/project-layout.md)
- [module resolution baseline](../guidelines/module-resolution.md)
- [type imports and import syntax](../guidelines/type-imports.md)
- [export style](../guidelines/export-style.md)
- [import paths and extensions](../guidelines/import-paths.md)
- [barrel files](../guidelines/barrel-files.md)
- [side-effect imports](../guidelines/side-effect-imports.md)
- [interface vs type](../guidelines/interface-vs-type.md)
- [enums](../guidelines/enums.md)
- [any, unknown, never](../guidelines/any-unknown-never.md)
- [assertions and satisfies](../guidelines/assertions-and-satisfies.md)
- [null and undefined](../guidelines/null-and-undefined.md)
- [readonly and immutability](../guidelines/readonly-and-immutability.md)
- [records and index signatures](../guidelines/records-and-index-signatures.md)
- [discriminated unions](../guidelines/discriminated-unions.md)
- [generics](../guidelines/generics.md)
- [variables and naming](../guidelines/variables-and-naming.md)
- [function form](../guidelines/function-form.md)
- [function signatures](../guidelines/function-signatures.md)
- [classes](../guidelines/classes.md)
- [decorators](../guidelines/decorators.md)
- [error handling](../guidelines/error-handling.md)
- [runtime validation](../guidelines/runtime-validation.md)
- [async patterns](../guidelines/async-patterns.md)
- [resource management](../guidelines/resource-management.md)
- [dates and modern stdlib](../guidelines/dates-and-stdlib.md)
- [test structure](../guidelines/test-structure.md)
- [test doubles](../guidelines/test-doubles.md)
- [type-level tests](../guidelines/type-level-tests.md)
- [comments and JSDoc](../guidelines/comments-and-jsdoc.md)
- [suppressions and escape hatches](../guidelines/suppressions.md)

## Workflow

1. Scope the review to the changed TypeScript files:

   ```sh
   git diff --name-only --diff-filter=d main... -- '*.ts' '*.tsx'
   ```

2. Run the mechanical gates first; they own everything a tool can decide, so
   review time goes to the rest:

   ```sh
   bun run typecheck
   bun run lint
   bun test
   ```

3. Read each changed file and check it against the grouped checklist below. Each
   item ends with a link to the guideline that owns the rule; open that page when
   a call is unclear.

### Config

- tsconfig strictness add-ons intact; no flag relaxed to make code compile ([tsconfig baseline](../guidelines/tsconfig-baseline.md)).
- biome.json opinionated rules and formatter defaults unchanged ([biome baseline](../guidelines/biome-baseline.md)).
- `types` array explicit; no wildcard `@types` discovery ([type packages](../guidelines/type-packages.md)).
- Exact version pins, Bun APIs over Node-portable code ([bun runtime and dependencies](../guidelines/bun-runtime-and-dependencies.md)).
- `src/` layer folders, kebab-case names, colocated `*.test.ts` ([project layout](../guidelines/project-layout.md)).

### Modules

- ESM only; no `require`, `module.exports`, `__dirname` ([module resolution](../guidelines/module-resolution.md)).
- Type-only imports use top-level `import type`; builtins use `node:` ([type imports](../guidelines/type-imports.md)).
- Named exports; default only where a framework requires it ([export style](../guidelines/export-style.md)).
- Relative and `#` subpath imports with `.ts` extensions; no `tsconfig` `paths` ([import paths](../guidelines/import-paths.md)).
- No barrel `index.ts` re-export files; no import cycles ([barrel files](../guidelines/barrel-files.md)).
- Side-effect imports only for real side effects; assets declared in a global `.d.ts` ([side-effect imports](../guidelines/side-effect-imports.md)).

### Types

- `interface` for object shapes, `type` for the rest ([interface vs type](../guidelines/interface-vs-type.md)).
- No `enum` or `const enum`; `as const` objects or literal unions ([enums](../guidelines/enums.md)).
- No `any` or boxed globals; `unknown` + narrowing at boundaries ([any, unknown, never](../guidelines/any-unknown-never.md)).
- No `as` or `!` except `as const`; `satisfies` instead ([assertions and satisfies](../guidelines/assertions-and-satisfies.md)).
- `undefined`-first; optional properties over `| undefined` ([null and undefined](../guidelines/null-and-undefined.md)).
- `readonly` on DTOs, props, and public returns; copy over in-place mutation ([readonly and immutability](../guidelines/readonly-and-immutability.md)).
- `Record`/index-signature/`Map` chosen correctly; indexed reads narrowed ([records and index signatures](../guidelines/records-and-index-signatures.md)).
- One-of shapes modeled as discriminated unions with a `type` field and `assertNever` ([discriminated unions](../guidelines/discriminated-unions.md)).
- Generics relate inputs to outputs; advanced type-level types stay behind named library aliases ([generics](../guidelines/generics.md)).

### Functions

- `const`-first, no `var`; Google naming scheme ([variables and naming](../guidelines/variables-and-naming.md)).
- `function` declarations at top level, arrows for callbacks ([function form](../guidelines/function-form.md)).
- Explicit return types on exported functions; options object over overloads ([function signatures](../guidelines/function-signatures.md)).
- Classes only for stateful/framework needs; `private` keyword, `override`, no parameter properties ([classes](../guidelines/classes.md)).
- No decorators ([decorators](../guidelines/decorators.md)).

### Errors, Async, and Data

- `Result` for expected failures, `throw` only for bugs; `catch (e: unknown)` + narrowing ([error handling](../guidelines/error-handling.md)).
- External data validated with a Zod schema at the boundary ([runtime validation](../guidelines/runtime-validation.md)).
- No floating promises; independent work uses `Promise.all`/`allSettled` ([async patterns](../guidelines/async-patterns.md)).
- `using`/`await using` for files, connections, and locks ([resource management](../guidelines/resource-management.md)).
- `Temporal` in new code; modern built-ins over utility deps ([dates and modern stdlib](../guidelines/dates-and-stdlib.md)).

### Tests

- `bun:test` with `test`/`describe`; no loops, no shared mutable state ([test structure](../guidelines/test-structure.md)).
- Real implementations or nullable fakes; mock only boundaries and restore them ([test doubles](../guidelines/test-doubles.md)).
- `expectTypeOf` used only for a library's exported public types ([type-level tests](../guidelines/type-level-tests.md)).

### Docs

- Why-comments only, no change narration; JSDoc only where a contract is non-obvious ([comments and JSDoc](../guidelines/comments-and-jsdoc.md)).
- Every `biome-ignore` and `@ts-expect-error` carries a written reason ([suppressions](../guidelines/suppressions.md)).

4. Write the findings into a dated file under `docs/agent/reviews/` in the repo
   under review, grouping each finding by the area above and citing the owner page:

   ```sh
   mkdir -p docs/agent/reviews
   ```

## Avoid

- Re-checking by hand what the gates already enforce instead of running them first.
- Flagging a call without citing the guideline page that owns the rule.
- Reviewing generated or vendored interop zones as if they were hand-written code ([suppressions](../guidelines/suppressions.md)).
- Writing the review anywhere other than `docs/agent/reviews/` in the reviewed repo.
