---
name: typescript-style-guide
description: Apply the project's TypeScript conventions when writing, reviewing, refactoring, configuring, or migrating TypeScript. Covers Bun-first tooling, modules, type modeling, API design, neverthrow errors, Zod boundaries, bun:test, and React, CLI, and monorepo overlays. Also use when setting up a project or adding a dependency.
---

# TypeScript Style Guide

Apply the loaded policy pages directly.

## Routing

### Workflows

| Task | Load |
| --- | --- |
| Start a new project or package | [setup workflow](workflows/setup-project.md) |
| Review TypeScript changes | [review workflow](workflows/review-typescript.md) |
| Add or update a dependency | [dependency workflow](workflows/add-dependency.md) |
| Modernize legacy TypeScript | [migration workflow](workflows/migrate-legacy.md) |

### Policy Fast Paths

| Task | Load |
| --- | --- |
| Configure TypeScript or Biome | [tsconfig](guidelines/tsconfig-baseline.md), [type checking](guidelines/type-check-workflow.md), [Biome](guidelines/biome-baseline.md) |
| Change modules, imports, or exports | [module resolution](guidelines/module-resolution.md), [type imports](guidelines/type-imports.md), [exports](guidelines/export-style.md), [import paths](guidelines/import-paths.md), [barrels](guidelines/barrel-files.md) |
| Model domain or boundary data | [interface vs type](guidelines/interface-vs-type.md), [discriminated unions](guidelines/discriminated-unions.md), [immutability](guidelines/readonly-and-immutability.md), [runtime validation](guidelines/runtime-validation.md) |
| Handle expected failure or async work | [error handling](guidelines/error-handling.md), [runtime validation](guidelines/runtime-validation.md), [async patterns](guidelines/async-patterns.md), [resource management](guidelines/resource-management.md) |
| Write tests | [test structure](guidelines/test-structure.md), [test doubles](guidelines/test-doubles.md), [type-level tests](guidelines/type-level-tests.md) |
| Write React or TSX | [React components](guidelines/react-components.md), [hooks and context](guidelines/react-hooks-and-context.md) |
| Write a CLI | [CLI overlay](guidelines/cli-scripts.md) |
| Work in a monorepo | [monorepo overlay](guidelines/monorepo.md) |
| Other TypeScript policy work | [guideline index](guidelines.md) |

## Core Behavior

- Workflows own multi-step procedures and route to their policy pages.
- Fast paths load only the directly linked owner pages.
- Use the guideline index only when no workflow or fast path matches.
- Load overlays only when their `Activation` section applies.
- Prefer concrete TypeScript guidance over language tutorials.
- Ask one focused question only when required project context is missing.
