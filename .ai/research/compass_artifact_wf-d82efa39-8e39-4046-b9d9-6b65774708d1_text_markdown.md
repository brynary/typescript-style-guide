# Topic Map for a TypeScript Style Guide Skill (TS 6, Bun, Biome, React 19)

## TL;DR
- This report proposes a **34-topic skill** organized into 8 categories, sized to the "one markdown page per topic, ~30–40 max" budget, with each topic carrying a short scope description and — where relevant — an explicit list of the convention decisions the drafting team must settle before writing.
- The single most consequential cross-cutting decision is **how prescriptive to be**: because TypeScript 6 flips defaults (strict on, ESM, ES2025 target) and Biome auto-enforces a large slice of formatting/lint conventions, many "style" questions are already answered by tooling — the skill should defer to Biome/tsconfig for those and spend its prose budget on the genuinely contested choices (enums, type-vs-interface, error handling, validation library, exports, file naming).
- Skill-authoring best practice (Anthropic) argues for short, greppable, example-driven pages that "state the rule, then the why," avoid walls of ALL-CAPS imperatives, and use progressive disclosure — which is why this map keeps each topic to one page and flags decision points rather than burying them.

## Key Findings

### The technology baseline as of July 2026
- **TypeScript 6.0** shipped **March 23, 2026** as the last JavaScript-based compiler release and a deliberate bridge to **TypeScript 7.0**. Daniel Rosenwasser, Principal Product Manager, stated in "Announcing TypeScript 6.0": *"TypeScript 6.0 is a unique release in that we intend for it to be the last release based on the current JavaScript codebase."* TS 6 changes defaults: `strict: true`, `module: esnext`, `target: es2025`, `types: []` (no auto `@types` discovery), and `rootDir` no longer inferred from source files. It deprecates options that become **hard errors** in TS 7. This matters for the skill because the tsconfig topic must target the new defaults, and enum/namespace/decorator guidance must anticipate the erasable-syntax direction of the ecosystem.
- **TypeScript 7.0** is the Go-native "Corsa" port (Microsoft first publicly announced the native port on March 11, 2025, when Anders Hejlsberg wrote the team had "begun work on a native port of the TypeScript compiler and tools"). TS 7.0 reached **RC on June 18, 2026**; Microsoft's plan per that RC post is to release 7.0 **"within the next month,"** with a stable programmatic API not landing until TypeScript 7.1. Per Microsoft's TS 7.0 Beta announcement (April 21, 2026), *"TypeScript 7.0 is often about 10 times faster than TypeScript 6.0"* — backed by Microsoft's published tsc benchmark of the VS Code codebase (~1.5M lines): 77.8s → 7.5s (~10.4x), with editor project-load ~9.6s → ~1.2s. These are Microsoft's own benchmarks, not independent.
- **Bun** is the runtime, package manager, and test runner. Key facts: workspaces via the `package.json` `workspaces` key, the `workspace:*` protocol, catalogs for shared dependency versions, `bun test` is Jest-compatible with `mock()`, `spyOn()`, `mock.module()`, `--preload`, a `vi` alias, and `bunfig.toml` `[test]` config. Bun can break tsconfig path aliases in monorepos — a real gotcha to document.
- **Biome** is the only linter/formatter (never ESLint). Biome 2.x out-of-the-box defaults: **tab** indentation, indentWidth 2, **lineWidth 80**, **double** quotes, `semicolons: always`, `trailingCommas: all`, `arrowParentheses: always`, `jsxQuoteStyle: double`. Only a subset of style rules are on by default (`noExplicitAny` = warn [suspicious], `useImportType` = warn [style], `useNodejsImportProtocol` = info [style], `noImplicitAnyLet` = error [suspicious]); the opinionated rules — `noNonNullAssertion`, `noParameterAssign`, `useNamingConvention`, `useFilenamingConvention`, `useConsistentArrayType`, `noEnum`, `noBarrelFile`, `noReExportAll`, `useConsistentTypeDefinitions` (nursery) — are **off by default** and must be explicitly enabled. Biome 2 adds partial type-aware linting (e.g., `noFloatingPromises`), multi-file analysis (`noImportCycles`, `noPrivateImports`), GritQL plugins, framework "domains" (react/solid/next/test/types, auto-enabled from `package.json`), monorepo nested config via `extends: "//"`, and moved `organizeImports` under the `assist.actions.source` system. Note: Biome v2 removed the linter `all` option.
- **React 19** (stable since **December 5, 2024**): `ref` as a regular prop — the official post says *"New function components will no longer need forwardRef, and we will be publishing a codemod to automatically update your components to use the new ref prop"* — plus the `use()` hook, Actions + `useActionState`/`useFormStatus`/`useOptimistic`, `<Context>` as provider, document metadata hoisting, and no need to `import React` for JSX. The React Compiler reduces manual `useMemo`/`useCallback`.

### Skill-authoring best practices that shape the topic list
- Anthropic's guidance: SKILL.md is a lean "table of contents" with metadata (name + description) pre-loaded; detail lives in referenced files loaded on demand (**progressive disclosure**). Keep pages concise because every loaded token competes with the task context.
- **State the rule, then the why** — Anthropic's skill-creator flags all-caps MUST/ALWAYS/NEVER as a "yellow flag"; reasoning lets the agent generalize to unanticipated cases. Match instruction freedom to task fragility ("control tuning").
- Community CLAUDE.md/AGENTS.md experience: keep files short (HumanLayer notes Claude Code will ignore CLAUDE.md content it deems irrelevant; the harness injects a "may or may not be relevant" reminder), prefer good/bad code examples, keep rules greppable, and let the formatter/linter enforce mechanics rather than spending instruction budget on them. For AI agents specifically: **explicitness over cleverness, greppability, and conventions that are consistently mechanical** (e.g., one export style, one file-naming case) matter more than aesthetic preference. HumanLayer explicitly recommends Biome as the auto-fixing linter to offload formatting from the agent's instructions.

## Details: Proposed Topic List (34 topics in 8 categories)

Each topic = one markdown page. **DECISION POINTS** are flagged where multiple legitimate conventions exist.

### Category A — Skill meta & how to use it (2 topics)

**A1. Skill overview & how the agent should apply it.** Purpose, scope, tech baseline (TS 6, Bun, Biome, React 19), and the golden rule: *defer mechanical formatting/lint to Biome and tsconfig; consult these pages for decisions tools don't make.* Points the agent at the right page per task.
- DECISION: How prescriptive overall — hard rules vs. defaults-with-escape-hatches. Recommendation: hard rules for greppable/mechanical choices, "prefer X because Y" for judgment calls.
- DECISION: Whether pages should include a machine-checkable "Biome/tsc already enforces this" banner to avoid redundant instructions.

**A2. Precedence, escape hatches & when to break the rules.** How to override a convention (inline `biome-ignore` with reason, `@ts-expect-error` with explanation), and how the skill interacts with `AGENTS.md`/`CLAUDE.md`.
- DECISION: Allowed suppression syntax and whether every suppression must carry a written justification.

### Category B — Project setup & configuration (6 topics)

**B1. tsconfig baseline.** The canonical strict config for TS 6: `strict`, plus `noUncheckedIndexedAccess`, `exactOptionalPropertyTypes`, `verbatimModuleSyntax`, `isolatedModules`, `moduleDetection: force`, `module: esnext`/`nodenext`, `target: es2025`, `skipLibCheck`, `noEmit` (Bun runs TS directly).
- DECISION: How strict beyond `strict` — which of `noUncheckedIndexedAccess`, `exactOptionalPropertyTypes`, `noPropertyAccessFromIndexSignature`, `noImplicitReturns`, `noUnusedLocals/Parameters` are mandatory vs. recommended. (`exactOptionalPropertyTypes` and `noUncheckedIndexedAccess` add real friction.)
- DECISION: `verbatimModuleSyntax` on/off (interacts with Biome `useImportType` and Bun/Node type-stripping).
- DECISION: Whether to enable `erasableSyntaxOnly` to keep code runnable by type-stripping runtimes (forbids enums, namespaces, parameter properties, legacy decorators).

**B2. Biome configuration.** The baseline `biome.json`: recommended rules on, formatter settings, `assist.actions.source.organizeImports`, VCS integration, which off-by-default style rules to turn on.
- DECISION: Accept Biome formatter defaults (**tab** indent, **double** quotes, lineWidth 80) or override (many teams set space/single/100–120). Recommendation for agents: accept defaults to reduce config surface, but this is a real choice.
- DECISION: Which opinionated rules to enable: `noNonNullAssertion`, `noParameterAssign`, `useNamingConvention`, `useFilenamingConvention`, `noBarrelFile`/`noReExportAll`, `noEnum`, `useConsistentTypeDefinitions`, `useConsistentArrayType`.

**B3. Runtime & package management with Bun.** `bun install`, `bun add`, `bunfig.toml`, lockfile, `bun run`, running TS directly, when to reach for `bun build`.
- DECISION: Pin dependency versions exactly vs. ranges; whether to commit to `catalog:` for shared versions.
- DECISION: Bun-first policy — always prefer Bun APIs (`Bun.file`, `Bun.serve`) over Node equivalents, or stay Node-portable.

**B4. Project structure & file/folder layout.** `src/` layout, where tests live, colocation of components, feature-folder vs. layer-folder organization.
- DECISION: Feature-based vs. type-based folder structure.
- DECISION: Test colocation (`foo.test.ts` next to `foo.ts`) vs. separate `tests/` directory.

**B5. Monorepo & workspaces.** Bun workspaces, `workspace:*`, catalogs, per-package tsconfig, root vs. nested Biome config (`extends: "//"`).
- DECISION: Package-manager workspaces + TS project references vs. tsconfig path aliases for internal linking (path aliases need a bundler/runtime resolver and can break under Bun).
- DECISION: Single root tsconfig vs. per-package with references.

**B6. Path aliases vs. relative imports.** `paths` in tsconfig, `#` subpath imports (Node/TS-supported), Bun's handling.
- DECISION: Path aliases (e.g., `@/…`) vs. relative imports vs. Node subpath imports (`#/…`). For agents: aliases improve greppability and moveability but add resolver config and can break with Bun/type-stripping; relative imports are universally portable.
- DECISION: If aliases, the exact prefix scheme and how deep.

### Category C — Core TypeScript language conventions (9 topics)

**C1. `type` vs `interface`.** When to use each.
- DECISION: interface-for-objects/type-for-everything (common community convention, `extends` + declaration merging) vs. type-only (consistent, better for unions/intersections). Biome `useConsistentTypeDefinitions` can enforce either (its default `style` converts to `interface`). Recommendation for agents: pick ONE and enforce mechanically.

**C2. Enums and enum alternatives (the canonical decision point).** Cover `enum`, `const enum`, union of string literals, and `as const` object + derived union.
- DECISION: The four-way choice. `const enum` has documented pitfalls and breaks isolatedModules/type-stripping; `enum` emits runtime code and is banned by `erasableSyntaxOnly`; union literals are zero-runtime but no iteration; `as const` object gives runtime iteration + literal union (`type X = (typeof X)[keyof typeof X]`). The 2026 trend strongly favors `as const` objects or plain unions. Recommendation: default to `as const` object (iterable) or string-literal union (type-only); avoid `enum`/`const enum`.

**C3. `any` vs `unknown` and type-safety escape hatches.** Policy on `any`, `unknown` at boundaries, `never`.
- DECISION: Ban `any` entirely (`noExplicitAny` → error) vs. warn. Policy for unavoidable `any` (must be `unknown` + narrowing?).

**C4. Type assertions & non-null.** `as`, `as const`, `!` non-null, `satisfies`.
- DECISION: Whether `as` casts are banned except `as const`/`as unknown`; whether `!` is forbidden (`noNonNullAssertion`); when to prefer `satisfies` over `as`.

**C5. Functions: declarations vs. arrow functions.** Top-level functions, callbacks, methods.
- DECISION: `function` declarations for top-level vs. arrow-const everywhere. For agents: greppability of `function foo`, hoisting, `this` semantics. Biome `arrowParentheses: always` is default.
- DECISION: Explicit return types on exported functions — required or inferred?

**C6. Variables, immutability & naming.** `const` vs `let`, naming conventions (camelCase, PascalCase types, CONSTANT_CASE).
- DECISION: Whether to enable `useNamingConvention` (off by default) and exact scheme.
- DECISION: `readonly`/`Readonly<>` policy and `as const` for literals.

**C7. Null vs undefined & optionality.** Which to use for absence; optional properties vs. `| undefined` (interacts with `exactOptionalPropertyTypes`).
- DECISION: Prefer `undefined` (TS-idiomatic) vs. `null`; whether to allow both.
- DECISION: `foo?: T` vs `foo: T | undefined` semantics under `exactOptionalPropertyTypes`.

**C8. Generics & advanced types.** When to introduce generics, utility types (`Pick`/`Omit`/`Partial`/`Record`), discriminated unions, template literal types.
- DECISION: Complexity ceiling — how clever is too clever for an AI-maintained codebase (explicitness over cleverness).
- DECISION: Discriminated-union conventions (the `kind`/`type` tag field name).

**C9. Modules, imports & exports.** `import type`, import ordering (Biome organizes), `node:` protocol, barrel files.
- DECISION: **Named vs. default exports** (named improves greppability/refactor safety and is generally recommended for agents; default is a React-component idiom in some ecosystems).
- DECISION: **Barrel files (`index.ts` re-exports)** — allow vs. ban (`noBarrelFile`/`noReExportAll`); barrels hurt tree-shaking and can cause cycles.
- DECISION: `import type` style — top-level `import type {}` vs inline `import { type X }` (interacts with `verbatimModuleSyntax` and Biome `useImportType` style option).

### Category D — Error handling, validation & data (4 topics)

**D1. Error handling: exceptions vs. Result types.** `throw`/`try-catch`, custom Error classes, `Result<T,E>` / neverthrow, Effect.
- DECISION: Exceptions (idiomatic, works with ecosystem) vs. Result types (explicit in signatures, better for agents to handle exhaustively) vs. hybrid ("throw for unexpected, Result for expected"). A Result convention is very agent-friendly (forces handling, `eslint-plugin-neverthrow`-style enforcement) but adds verbosity and ecosystem friction.
- DECISION: Custom error taxonomy and whether `catch (e: unknown)` narrowing is mandatory.

**D2. Runtime validation & schema library.** Validating external data (API, env, forms).
- DECISION: **Zod vs. Valibot vs. ArkType** (vs. none). Zod is the ecosystem default (deepest tRPC/React Hook Form/Drizzle integration); **Valibot wins bundle size — a login-form schema is 1.37 KB with Valibot vs 17.7 KB with Zod standard and 6.88 KB with Zod Mini (esbuild, per Valibot's comparison docs)** for frontends; ArkType is fastest and most TS-native but has a string-DSL learning curve and weaker editor support. The Standard Schema spec now lets them interop. Recommendation: pick one primary; Zod as safe default, Valibot if frontend bundle-critical.
- DECISION: Where validation lives (boundaries only) and inferring types from schemas vs. hand-writing.

**D3. Dates, numbers & data primitives.** `Temporal` (now typed in the TS 6 es2025 lib), money/decimal handling, `Date` policy.
- DECISION: Adopt `Temporal` vs. stick with `Date`/library; decimal strategy.

**D4. Async patterns.** Promises, `async/await`, cancellation (`AbortSignal`), concurrency (`Promise.all`/`allSettled`), `using`/`await using` (TS 6 explicit resource management).
- DECISION: Whether to adopt `using` for resource cleanup.
- DECISION: Error propagation in async (ties to D1).

### Category E — React & frontend (6 topics)

**E1. Component typing & definition.** Function components, typing props, `React.FC` vs plain function.
- DECISION: **`React.FC` vs plain function** with typed props (community trend away from `React.FC`; plain function preferred).
- DECISION: `interface` vs `type` for props (should mirror C1); naming (`FooProps`).
- DECISION: Whether `import React` is banned now that JSX doesn't require it.

**E2. Hooks typing.** `useState`/`useRef`/`useReducer`/`useContext` typing, custom hooks, `useActionState`/`useFormStatus` (React 19).
- DECISION: When to annotate `useState<T>()` explicitly vs. infer.
- DECISION: Custom-hook return shape (tuple vs object) and `as const` for tuples.

**E3. Props patterns.** `children` typing (`ReactNode`), `ComponentProps<'button'>`/`ComponentPropsWithRef`, `ref` as prop (React 19), spreading props.
- DECISION: Ref forwarding style — React 19 `ref`-as-prop vs. legacy `forwardRef` (for libraries still supporting React <19).
- DECISION: Extending native element props policy.

**E4. State management typing.** Local vs. global state, typing Context, and external stores (Zustand/Redux Toolkit/Jotai/etc.).
- DECISION: Which state library (if any) is standard, and its typing patterns.
- DECISION: Context typing convention (the `createContext(undefined)` + guard-hook pattern).

**E5. Styling & JSX conventions.** JSX formatting (Biome-owned), className strategy, conditional classes, inline style typing (`CSSProperties`).
- DECISION: Styling approach (Tailwind / CSS Modules / CSS-in-JS) and the `cn()`-style helper.
- DECISION: `jsxQuoteStyle` (Biome default double).

**E6. Data fetching & Server/Client components.** `use()` for promises, Suspense boundaries, TanStack Query typing, RSC/`'use client'` boundaries (if applicable).
- DECISION: Whether RSC/Server Actions are in scope or client-only SPA (affects whether this topic is needed — candidate to cut if pure SPA).
- DECISION: Data-fetching library standard and its typing.

### Category F — Testing (3 topics)

**F1. Test structure & the Bun test runner.** `bun test`, file naming (`*.test.ts`), `describe`/`test`, lifecycle hooks, `bunfig.toml` `[test]`, assertions.
- DECISION: Test file naming and location (mirrors B4 colocation decision).
- DECISION: `test` vs `it`; assertion style.

**F2. Mocking & test doubles.** `mock()`, `spyOn()`, `mock.module()` + `--preload`, `vi` alias, fake timers, restoring mocks in `afterEach`.
- DECISION: Mocking philosophy (mock at boundaries only vs. liberal); module-mock preload conventions.

**F3. React component & DOM testing.** Testing Library with Bun's DOM testing (happy-dom/jsdom), interaction testing, what to test.
- DECISION: DOM environment (happy-dom vs jsdom) and Testing Library setup under Bun.
- DECISION: Snapshot testing policy.

### Category G — CLI scripts & tooling (2 topics)

**G1. CLI script conventions.** Bun-executed scripts: shebang/entry, arg parsing, `Bun.argv`, exit codes, stdout/stderr, env access.
- DECISION: Arg-parsing approach (built-in `util.parseArgs` vs. library).
- DECISION: Structured logging vs. `console`; script error-exit conventions.

**G2. Build & distribution.** `bun build` for bundling, when a build step is needed vs. running TS directly, `Vite` for React dev/HMR.
- DECISION: When Vite is introduced (React HMR/dev server) vs. Bun-only.
- DECISION: Library output (dts emit, `exports` map) if publishing.

### Category H — Cross-cutting quality (2 topics)

**H1. Comments, JSDoc & documentation.** When to comment (the "why" not "what"), JSDoc on exported APIs, TODO conventions.
- DECISION: JSDoc required on public/exported symbols vs. optional.
- DECISION: Whether comments explaining agent decisions are encouraged.

**H2. Code examples appendix (good/bad patterns).** A reference page of canonical good-vs-bad snippets the agent can pattern-match against — the highest-value page for AI adherence.
- DECISION: Which patterns are worth canonical examples (likely enums, error handling, exports, component typing).

## Recommendations

**Staging the drafting work:**
1. **Settle the ~15 highest-impact decision points first** (they gate multiple pages): tsconfig strictness level; Biome formatter defaults vs. overrides; type vs interface; enum strategy; any/unknown; exports (named vs default) + barrels; error handling (exceptions vs Result); validation library; React.FC vs plain function; state library; path aliases vs relative; test colocation. Everything else follows.
2. **Write B1 (tsconfig) and B2 (Biome) early** and treat them as the source of truth; every other page should say "enforced by Biome/tsc — see B2/B1" instead of restating mechanics. This maximizes prose budget for real decisions and follows the skill-authoring principle of not spending tokens on what tooling already guarantees.
3. **Write H2 (good/bad examples) last but treat it as first-class** — for AI agents, a greppable pattern library drives adherence more than prose.

**Scoping / cut-or-merge guidance to stay in budget (34 is within 30–40, but if it runs long):**
- **Merge candidates:** C6 (variables/naming) into C5 as a single "identifiers & immutability" page; D3 (dates/numbers) could fold into D2 or C8; H1 (comments) could merge into A2 or a general conventions page.
- **Cut candidates if pure SPA:** E6 (Server/Client components) collapses to just `use()` + data fetching; G2 could merge into B3 if not publishing libraries.
- **Do not merge** the flagship decision pages (C1 type-vs-interface, C2 enums, C9 exports/barrels, D1 error handling, D2 validation, E1 component typing) — these are the reason the skill exists.

**Authoring style for each page (apply Anthropic guidance):**
- Keep each page to one screen; lead with the rule, follow with a one-line "why," then a good example and a bad example.
- Avoid ALL-CAPS imperative walls; reserve strong "never" only for genuinely dangerous patterns.
- Make rules greppable and mechanical wherever possible (one export style, one file-naming case) so agents apply them consistently.
- Use progressive disclosure: the overview page (A1) routes to detailed pages; don't inline everything.

## Caveats
- **Version churn risk:** TS 7 (Go compiler) is in RC (June 18, 2026) and Microsoft plans to ship it "within the next month"; when it lands it turns TS 6 deprecations into hard errors and removes some options (stable programmatic API waits for TS 7.1). The tsconfig and enum/decorator pages should be written to be TS 7-forward (favor erasable syntax). Cited performance figures (~10x faster; validation bundle sizes) are Microsoft/vendor benchmarks, not independent measurements.
- **Biome defaults can shift:** nursery rules are unstable and rule recommendation status changes between minor versions; the Biome page should pin a Biome version and note that nursery rules (e.g., `useConsistentTypeDefinitions`) require explicit opt-in, and that Biome v2 removed the linter `all` option.
- **Bun ecosystem gaps:** Bun's Jest compatibility is not 100% (some `jest.mock`/timer features historically lagged), and Bun can break tsconfig path aliases in monorepos — both should be documented as known gotchas rather than assumed working.
- **Contested choices are genuinely contested:** type-vs-interface, named-vs-default exports, exceptions-vs-Result, and Zod-vs-Valibot-vs-ArkType have no universal "right" answer; this report deliberately surfaces them as decisions rather than prescribing, per the task's emphasis on mapping terrain.
- This report maps topics and decisions; it does **not** itself constitute the style guide, and the final per-topic prescriptions remain to be written in the separate drafting phase.