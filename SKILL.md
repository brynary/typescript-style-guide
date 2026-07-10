---
name: typescript-style-guide
description: Apply the project's TypeScript conventions when writing, reviewing, refactoring, configuring, or migrating TypeScript. Covers Bun-first tooling, modules, type modeling, API design, neverthrow errors, Zod boundaries, bun:test, and React, CLI, and monorepo overlays. Also use when setting up a project or adding a dependency.
---

# TypeScript Style Guide

Apply the loaded policy pages directly; do not treat this router as the guide itself.

## Routing Examples

| Task | Load |
| --- | --- |
| Write or refactor TypeScript code | [guidelines.md](guidelines.md), then matching pages |
| Create or edit tsconfig.json / biome.json | [guidelines.md](guidelines.md), then the config baselines |
| Start a new project or package | [workflows/setup-project.md](workflows/setup-project.md) |
| Review a TypeScript diff or PR | [workflows/review-typescript.md](workflows/review-typescript.md) |
| Add or update a dependency | [workflows/add-dependency.md](workflows/add-dependency.md) |
| Modernize legacy TypeScript | [workflows/migrate-legacy.md](workflows/migrate-legacy.md) |
| React, CLI, or monorepo work | [guidelines.md](guidelines.md), then matching overlays |

## Core Behavior

- Load only the pages the task needs; guideline pages are the policy, workflow pages are the procedures.
- Overlay pages (React, CLI, monorepo) declare when they apply in their `Activation` section; skip them otherwise.
- Prefer concrete TypeScript guidance over language tutorials.
- Apply the loaded rules directly. Ask one focused question only when required project context is missing.
