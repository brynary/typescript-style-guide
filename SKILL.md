---
name: typescript-style-guide
description: Apply this TypeScript style guide when writing, reviewing, refactoring, or configuring TypeScript code. Covers Bun-first tooling (tsconfig, Biome, bun:test), modules and imports, type modeling (interface vs type, no enums, no any/as/!), functions and classes, error handling with neverthrow, Zod validation, testing, comments, and React/CLI/monorepo overlays. Also use for setting up a new project, reviewing TypeScript changes, adding a dependency, or migrating legacy code to the guide.
---

# TypeScript Style Guide

Use this skill to apply the project's TypeScript style conventions while writing, reviewing, refactoring, or configuring TypeScript code.

## Supporting Files

- [guidelines.md](guidelines.md) - index of TypeScript style policy pages. Load this for ordinary TypeScript work, then load only the guideline pages relevant to the task.
- [workflows/setup-project.md](workflows/setup-project.md) - scaffold a new Bun + TypeScript + Biome project on the guide's baselines.
- [workflows/review-typescript.md](workflows/review-typescript.md) - checklist-driven review pass over TypeScript changes.
- [workflows/add-dependency.md](workflows/add-dependency.md) - vet, pin, and install a dependency safely.
- [workflows/migrate-legacy.md](workflows/migrate-legacy.md) - bring existing code up to the guide.

## Routing Examples

| Task | Load |
| --- | --- |
| Write or refactor TypeScript code | [guidelines.md](guidelines.md), then the matching pages |
| Create or edit tsconfig.json / biome.json | [guidelines.md](guidelines.md) -> tsconfig and Biome baselines |
| Start a new project or package | [workflows/setup-project.md](workflows/setup-project.md) |
| Review a TypeScript diff or PR | [workflows/review-typescript.md](workflows/review-typescript.md) |
| Add or update a dependency | [workflows/add-dependency.md](workflows/add-dependency.md) |
| Modernize legacy TypeScript | [workflows/migrate-legacy.md](workflows/migrate-legacy.md) |
| React, CLI, or monorepo work | [guidelines.md](guidelines.md) -> the matching overlay pages |

## Core Behavior

- Load only the pages the task needs; guideline pages are the policy, workflow pages are the procedures.
- Overlay pages (React, CLI, monorepo) declare when they apply in their `Activation` section; skip them otherwise.
- Prefer concrete TypeScript guidance over language tutorials.
- Apply the loaded rules directly. Ask one focused question only when required project context is missing.
