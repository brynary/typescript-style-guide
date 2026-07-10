# Policy Ownership Map

This map records the owner page for each policy area and the decision rows that authorize it. The current task router is [guidelines.md](guidelines.md); use this file only when maintaining policy.

## Foundations and Toolchain

| Owner page | Decisions |
| --- | --- |
| [tsconfig baseline](guidelines/tsconfig-baseline.md) | CFG-1, CFG-2, CFG-3, SCOPE-4 |
| [Type-check workflow](guidelines/type-check-workflow.md) | CFG-4 |
| [Type packages](guidelines/type-packages.md) | CFG-5 |
| [Biome baseline](guidelines/biome-baseline.md) | CFG-6, CFG-7 |
| [Suppressions](guidelines/suppressions.md) | CFG-8 |
| [Bun runtime and dependencies](guidelines/bun-runtime-and-dependencies.md) | CFG-9, CFG-10 |
| [Project layout](guidelines/project-layout.md) | CFG-11, CFG-12, CFG-13 |

## Modules, Imports, and Exports

| Owner page | Decisions |
| --- | --- |
| [Module resolution](guidelines/module-resolution.md) | MOD-1 |
| [Type imports](guidelines/type-imports.md) | MOD-2 |
| [Export style](guidelines/export-style.md) | MOD-3, MOD-8 |
| [Import paths](guidelines/import-paths.md) | MOD-4, MOD-5 |
| [Barrel files](guidelines/barrel-files.md) | MOD-6 |
| [Side-effect imports](guidelines/side-effect-imports.md) | MOD-7 |

## Type Modeling

| Owner page | Decisions |
| --- | --- |
| [Interface vs type](guidelines/interface-vs-type.md) | TYPE-1 |
| [Enums](guidelines/enums.md) | TYPE-2 |
| [any, unknown, never](guidelines/any-unknown-never.md) | TYPE-3 |
| [Assertions and satisfies](guidelines/assertions-and-satisfies.md) | TYPE-4 |
| [Null and undefined](guidelines/null-and-undefined.md) | TYPE-5 |
| [Readonly and immutability](guidelines/readonly-and-immutability.md) | TYPE-6 |
| [Records and index signatures](guidelines/records-and-index-signatures.md) | CFG-1 |
| [Discriminated unions](guidelines/discriminated-unions.md) | TYPE-7 |
| [Generics](guidelines/generics.md) | TYPE-8 |

## Functions, Classes, and API Design

| Owner page | Decisions |
| --- | --- |
| [Variables and naming](guidelines/variables-and-naming.md) | FUN-1 |
| [Function form](guidelines/function-form.md) | FUN-2 |
| [Function signatures](guidelines/function-signatures.md) | FUN-3 |
| [Classes](guidelines/classes.md) | FUN-4, FUN-5, CFG-2 |
| [Decorators](guidelines/decorators.md) | FUN-6 |

## Errors, Async, and Data

| Owner page | Decisions |
| --- | --- |
| [Error handling](guidelines/error-handling.md) | ERR-1 |
| [Runtime validation](guidelines/runtime-validation.md) | ERR-2, ERR-6 |
| [Async patterns](guidelines/async-patterns.md) | ERR-1, ERR-5 |
| [Resource management](guidelines/resource-management.md) | ERR-4 |
| [Dates and modern stdlib](guidelines/dates-and-stdlib.md) | ERR-3, ERR-5 |

## Testing and Documentation

| Owner page | Decisions |
| --- | --- |
| [Test structure](guidelines/test-structure.md) | TEST-1, CFG-12 |
| [Test doubles](guidelines/test-doubles.md) | TEST-2 |
| [Type-level tests](guidelines/type-level-tests.md) | TEST-3 |
| [Comments and JSDoc](guidelines/comments-and-jsdoc.md) | DOC-1 |

## Conditional Overlays

| Owner page | Decisions |
| --- | --- |
| [React components](guidelines/react-components.md) | SCOPE-3, REACT-1, REACT-2 |
| [React hooks and context](guidelines/react-hooks-and-context.md) | SCOPE-3, REACT-2, REACT-3 |
| [CLI scripts](guidelines/cli-scripts.md) | SCOPE-3, CFG-9 |
| [Monorepo](guidelines/monorepo.md) | SCOPE-3, CFG-4, CFG-5, CFG-10, MOD-1, MOD-4 |

## Workflows

| Procedure | Purpose |
| --- | --- |
| [Set up a project](workflows/setup-project.md) | Scaffold the canonical Bun, TypeScript, and Biome baseline. |
| [Review TypeScript](workflows/review-typescript.md) | Run gates, route changed concerns to owner pages, and report findings. |
| [Add a dependency](workflows/add-dependency.md) | Vet, pin, trust, and verify a dependency. |
| [Migrate legacy code](workflows/migrate-legacy.md) | Adopt the baseline in dependency-ordered stages. |
