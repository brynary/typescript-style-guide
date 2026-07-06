# Guidelines

Load this file for TypeScript style policy, then load only the guideline pages needed for the task.

Guideline pages are policy. Do not load every guideline page by default. Overlay pages (React, CLI, monorepo) carry an `Activation` section; load them only when it matches the task.

## Foundations and Toolchain

- [tsconfig baseline](guidelines/tsconfig-baseline.md) - creating or editing tsconfig.json; the canonical strict TS 6 compiler config.
- [Type-check workflow](guidelines/type-check-workflow.md) - running or wiring type checks; Bun and Vite do not type-check, `tsc --noEmit` does.
- [Type packages](guidelines/type-packages.md) - adding @types packages or editing the `types` array.
- [Biome baseline](guidelines/biome-baseline.md) - creating or editing biome.json; formatter defaults and the enabled lint rules.
- [Suppressions](guidelines/suppressions.md) - writing any `biome-ignore` or `@ts-expect-error`.
- [Bun runtime and dependencies](guidelines/bun-runtime-and-dependencies.md) - choosing Bun vs Node APIs, installing or pinning dependencies.
- [Project layout](guidelines/project-layout.md) - creating files or folders; layer folders, kebab-case names, colocated tests.

## Modules, Imports, and Exports

- [Module resolution](guidelines/module-resolution.md) - configuring module/moduleResolution; ESM-only policy.
- [Type imports](guidelines/type-imports.md) - importing types or Node builtins; top-level `import type`.
- [Export style](guidelines/export-style.md) - exporting anything; named exports only.
- [Import paths](guidelines/import-paths.md) - writing import specifiers; relative vs `#` subpaths, always `.ts` extensions.
- [Barrel files](guidelines/barrel-files.md) - tempted to add an index.ts re-export; barrels are banned.
- [Side-effect imports](guidelines/side-effect-imports.md) - importing CSS/assets or bare side-effect modules.

## Type Modeling

- [Interface vs type](guidelines/interface-vs-type.md) - declaring a named type; `interface` for object shapes, `type` otherwise.
- [Enums](guidelines/enums.md) - modeling a fixed set of values; `as const` objects and literal unions, never `enum`.
- [any, unknown, never](guidelines/any-unknown-never.md) - handling untyped or boundary data; `any` is banned.
- [Assertions and satisfies](guidelines/assertions-and-satisfies.md) - tempted to write `as` or `!`; use `satisfies` and `as const`.
- [Null and undefined](guidelines/null-and-undefined.md) - modeling absent values; undefined-first.
- [Readonly and immutability](guidelines/readonly-and-immutability.md) - declaring DTOs, props, or return types; readonly by default.
- [Records and index signatures](guidelines/records-and-index-signatures.md) - typing dictionaries and keyed lookups under noUncheckedIndexedAccess.
- [Discriminated unions](guidelines/discriminated-unions.md) - modeling variants or states; `type` discriminant and assertNever.
- [Generics](guidelines/generics.md) - writing generic or advanced types; the complexity ceiling.

## Functions, Classes, and API Design

- [Variables and naming](guidelines/variables-and-naming.md) - naming values, functions, types, and constants; const-first.
- [Function form](guidelines/function-form.md) - choosing `function` declarations vs arrows.
- [Function signatures](guidelines/function-signatures.md) - designing parameters and return types; explicit returns on exports.
- [Classes](guidelines/classes.md) - deciding whether a class is warranted and how to write one.
- [Decorators](guidelines/decorators.md) - tempted to use decorators; they are banned outside framework contracts.

## Errors, Async, and Data

- [Error handling](guidelines/error-handling.md) - any fallible operation; neverthrow Result for expected failures, throw for bugs.
- [Runtime validation](guidelines/runtime-validation.md) - ingesting external data (API, env, forms); Zod at the boundary.
- [Async patterns](guidelines/async-patterns.md) - writing async code; await discipline, concurrency, AbortSignal.
- [Resource management](guidelines/resource-management.md) - opening files, connections, or locks; `using` / `await using`.
- [Dates and modern stdlib](guidelines/dates-and-stdlib.md) - date/time work or utility helpers; Temporal and native stdlib first.

## Testing

- [Test structure](guidelines/test-structure.md) - writing bun:test tests; `test` + `describe`, no loops, no shared mutable state.
- [Test doubles](guidelines/test-doubles.md) - tempted to mock; minimal mocks, real implementations first.
- [Type-level tests](guidelines/type-level-tests.md) - library packages exporting public types; expectTypeOf.

## Documentation

- [Comments and JSDoc](guidelines/comments-and-jsdoc.md) - writing comments or JSDoc; why-comments only, minimal JSDoc.

## Overlays

- [React components](guidelines/react-components.md) - writing React components or .tsx; plain functions, typed props, Tailwind.
- [React hooks and context](guidelines/react-hooks-and-context.md) - typing hooks, custom hooks, or context in React code.
- [CLI scripts](guidelines/cli-scripts.md) - writing CLI tools or scripts; Bun Shell, parseArgs, exit codes.
- [Monorepo](guidelines/monorepo.md) - working in a Bun workspace; catalogs, per-package tsconfig, nested Biome.
