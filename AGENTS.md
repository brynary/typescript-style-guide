# AGENTS.md

## Project Purpose

This repository is a TypeScript style guide packaged as a skill for AI coding agents. The guide should help agents write idiomatic TypeScript using the project owner's conventions.

The guide is drafted: the decision register is resolved and all guideline, overlay, and workflow pages exist. Ongoing work is maintenance.

Keep the work simple, explicit, and useful for agents. Do not turn the guide into a TypeScript textbook.

## Key Files

- [authoring/DRAFTING.md](authoring/DRAFTING.md): maintenance entrypoint for decisions, ownership, and page shape.
- [SKILL.md](SKILL.md): skill entrypoint and root router.
- [guidelines.md](guidelines.md): guideline index for progressive disclosure.
- [guidelines/](guidelines): focused TypeScript style policy pages.
- [workflows/](workflows): procedural workflows for larger tasks.
- [checks/](checks): validation harness for the packaged skill.
- [.ai/research/](.ai/research): source research reports used to create the outline.

## Working Rules

- Follow [authoring/DRAFTING.md](authoring/DRAFTING.md) when maintaining the guide; it routes to the decision register, ownership map, and page template.
- Keep guideline pages short, concrete, and mechanical enough for an agent to follow.
- Keep [SKILL.md](SKILL.md) small. Put detailed policy in `guidelines/` and procedures in `workflows/`.
- Link each workflow exactly once from [SKILL.md](SKILL.md) and each guideline exactly once from [guidelines.md](guidelines.md); use direct guideline links in `SKILL.md` only for policy fast paths.
- Run `bash checks/check.sh` before committing skill changes.
- Do not edit files in [.ai/research/](.ai/research) unless explicitly asked.

## Style Guide Bias

The decision register is fully resolved (2026-07-06). The guide's posture:

- Hard rules for mechanical choices; reasoned defaults for judgment calls.
- Maximum strictness: `strict` plus every add-on flag (including `noUncheckedIndexedAccess` and `exactOptionalPropertyTypes`); `erasableSyntaxOnly` on, so no enums, namespaces, or constructor parameter properties.
- Bun-first (runtime, package manager, test runner); Biome with default formatter settings; `tsc --noEmit` as the type-check gate.
- ESM only; named exports only; barrel files banned; relative imports locally and `#` subpath imports across areas, always with `.ts` extensions.
- `interface` for object shapes, `type` otherwise; `undefined`-first absence model; `readonly` by default; no `any`, `as`, or `!` in production code (`as const` allowed; prefer `satisfies`).
- Errors: neverthrow `Result` for expected failures, `throw` only for bugs; Zod validation at boundaries; Temporal over `Date` in new code.
- `function` declarations top-level with arrows for callbacks; explicit return types on exported functions; function-first, classes only for stateful abstractions.
- Tests: bun:test with `test` + `describe`, colocated `*.test.ts`, minimal mocks (real implementations or nullables; mock only process boundaries).
- Layer folders (`routes/`, `services/`, `models/`); kebab-case file names.
- Overlays in scope: React/TSX (Tailwind for styling; state and data fetching per-project), CLI scripts, monorepo/workspaces. Package publishing is out of scope.

## Editing Expectations

- Preserve ASCII-only markdown unless a file already uses non-ASCII intentionally.
- Keep changes narrowly scoped to the requested document.
- Avoid adding new guideline or workflow files unless the user asks for new pages.
- Do not include planning docs or research reports in the packaged skill unless explicitly requested.
