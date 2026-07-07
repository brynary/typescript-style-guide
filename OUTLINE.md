# TypeScript Style Guide Skill Outline

This outline is for a TypeScript style guide packaged as an AI-agent skill. The skill should not teach TypeScript syntax or re-explain the language handbook. It should give agents clear defaults, exceptions, and small examples for the decisions they face while writing TypeScript code.

Target size: proposed 36 core guideline pages plus conditional overlay pages and 4 workflow pages (SCOPE-2 in [DECISIONS.md](DECISIONS.md)); each guideline should fit on one focused markdown page.

## Technology Baseline

Synthesized from the research reports in `.ai/research/` (July 2026):

- TypeScript 6 (strict by default, `module: esnext`, `target: es2025`, `types: []`, `rootDir` no longer inferred), written TS 7-forward: favor erasable syntax, avoid options TS 7 removes.
- Bun as runtime, package manager, and test runner. Bun and Vite transpile only; type checking is a separate `tsc --noEmit` step.
- Biome 2.x as the only linter and formatter (never ESLint/Prettier). Pages defer mechanical formatting and lint rules to Biome and tsconfig; prose budget goes to decisions tooling cannot make.
- Vite for client-side dev/HMR where a frontend exists; React 19 only if the React overlay is in scope (SCOPE-3).

## Drafting Instructions

Use [DRAFTING.md](DRAFTING.md) for drafting order, scope, and page-writing guidance.

## Page Template

Use [TEMPLATE.md](TEMPLATE.md) for every guideline page.

## Packaged Skill Shape

The final skill should follow the progressive-disclosure pattern in [SKILL.md](SKILL.md) and [guidelines.md](guidelines.md): a small root router, focused guideline pages, and task workflows under `workflows/`. Planning files and research reports should not be part of the packaged skill by default.

## Decision Register

Use [DECISIONS.md](DECISIONS.md) to resolve style decisions before drafting final policy pages.

## Guideline Map

Numbered topics grouped into sections. Decision references point at [DECISIONS.md](DECISIONS.md) rows; a page cannot be drafted until its referenced rows are resolved.

### Section 1: Foundations and Toolchain

1. **tsconfig baseline**
   - Canonical strict compiler config for TS 6; strictness add-ons; `erasableSyntaxOnly`; `verbatimModuleSyntax`; `noEmit` (Bun runs TS directly).
   - Source of truth for compiler mechanics; later pages link here instead of restating flags.
   - Decisions: CFG-1, CFG-2, CFG-3, SCOPE-4.
2. **Type-check workflow**
   - Bun and Vite strip types without checking; when the agent must run `tsc --noEmit` and which tsconfig is authoritative.
   - Decisions: CFG-4.
3. **Type packages and globals**
   - Explicit `types` arrays per package (`["bun"]`, `["react"]`); no wildcard `@types` discovery.
   - Decisions: CFG-5.
4. **Biome baseline and rule ownership**
   - Canonical `biome.json`; formatter settings; which opinionated rules are enabled; what Biome owns vs what the guide's prose owns.
   - Source of truth for lint/format mechanics; later pages link here instead of restating rules.
   - Decisions: CFG-6, CFG-7.
5. **Suppressions and escape hatches**
   - `biome-ignore` and `@ts-expect-error` usage, required justifications, and how generated/legacy/interop zones are isolated and marked.
   - Decisions: CFG-8.
6. **Bun runtime and dependencies**
   - `bun install`/`bun run`, lockfile, `bunfig.toml`; Bun APIs vs Node-portable code; dependency pinning and catalogs.
   - Decisions: CFG-9, CFG-10.
7. **Project layout and file naming**
   - `src/` layout, feature vs layer folders, file naming case, test file placement.
   - Decisions: CFG-11, CFG-12, CFG-13.

### Section 2: Modules, Imports, and Exports

8. **Module resolution baseline**
   - ESM only; `preserve` + `bundler` semantics; when `nodenext` is permitted.
   - Decisions: MOD-1.
9. **Type imports and import syntax**
   - `import type` style under `verbatimModuleSyntax`; `node:` protocol; import ordering is Biome-owned.
   - Decisions: MOD-2.
10. **Export style**
    - Named exports; minimal export surface; the narrow cases where defaults are tolerated.
    - Decisions: MOD-3, MOD-8.
11. **Import paths and extensions**
    - Relative vs `#` subpath imports vs `tsconfig` `paths`; local import extension style; import attributes (`with`) for JSON/asset loaders.
    - Decisions: MOD-4, MOD-5.
12. **Barrel files and public API surface**
    - Barrel policy; curating the public surface with `package.json` `exports`; no import cycles.
    - Decisions: MOD-6.
13. **Side-effect imports and asset modules**
    - `noUncheckedSideEffectImports` handling; wildcard ambient declarations for CSS/SVG/assets.
    - Decisions: MOD-7.

### Section 3: Type Modeling

14. **Interface vs type**
    - The one house rule and its enforcement via Biome.
    - Decisions: TYPE-1.
15. **Enums and their replacements**
    - `as const` objects and literal unions instead of `enum`/`const enum`; the derived-union idiom.
    - Decisions: TYPE-2 (cascades from CFG-2).
16. **any, unknown, never, and banned type spellings**
    - `unknown` + narrowing at trust boundaries; `never` for impossibility; ban `Object`, `String`, `Function`, bare `{}`.
    - Decisions: TYPE-3.
17. **Assertions, satisfies, and literal inference**
    - When to use `as const`, `satisfies`, and `const` type parameters; policy on `as` and `!`.
    - Decisions: TYPE-4.
18. **Null, undefined, and optionality**
    - The absence model; optional properties vs `| undefined` under `exactOptionalPropertyTypes`.
    - Decisions: TYPE-5.
19. **Readonly and immutability**
    - `const` bindings, `readonly` properties, `Readonly<T>`; copy-and-return over in-place mutation.
    - Decisions: TYPE-6.
20. **Dictionaries, Record, and index signatures**
    - `Record` vs index signatures vs `Map`; safe unknown-key access under `noUncheckedIndexedAccess`.
    - Decisions: none of its own; applies CFG-1.
21. **Discriminated unions, narrowing, and exhaustiveness**
    - Discriminated unions over boolean flag bags; the standard discriminant field; guard helpers and the `assertNever` idiom.
    - Decisions: TYPE-7.
22. **Generics and type-level complexity**
    - Type parameter naming; when generics earn their place; mapped/conditional/template-literal ceiling.
    - Decisions: TYPE-8.

### Section 4: Functions, Classes, and API Design

23. **Variables and naming**
    - `const`-first, no `var`; the naming scheme across values, types, constants, and files.
    - Decisions: FUN-1.
24. **Function form**
    - Declarations vs arrows; `this` rules; when each form is required.
    - Decisions: FUN-2.
25. **Signatures: return types, overloads, and parameters**
    - Explicit return type policy; unions or options objects before overloads; parameter object threshold, destructuring, defaults.
    - Decisions: FUN-3.
26. **Classes and visibility**
    - Function-first policy; visibility keywords; `override`; no constructor parameter properties (cascades from CFG-2).
    - Decisions: FUN-4, FUN-5.
27. **Decorators**
    - Whether decorators appear at all; standard vs legacy modes.
    - Decisions: FUN-6.

### Section 5: Errors, Async, and Data

28. **Error handling model**
    - Exceptions vs Result types vs hybrid; error taxonomy; `catch (e: unknown)` narrowing.
    - Decisions: ERR-1.
29. **Runtime validation at boundaries**
    - Validating external data (API, env, forms); schema-derived types instead of hand-written ones; generated types for machine-readable contracts.
    - Decisions: ERR-2, ERR-6.
30. **Async patterns and cancellation**
    - `async`/`await` defaults; `Promise.all`/`allSettled`; `AbortSignal`; error propagation ties to the error model.
    - Decisions: applies ERR-1, ERR-5.
31. **Resource management**
    - `using` / `await using` with `Symbol.dispose` for files, connections, locks.
    - Decisions: ERR-4.
32. **Dates, Temporal, and modern stdlib**
    - Temporal over `Date`; native `Map.getOrInsert`, `RegExp.escape`, `Promise.try`.
    - Decisions: ERR-3, ERR-5.

### Section 6: Testing

33. **Test structure and runner**
    - bun:test conventions, file naming and placement, lifecycle hooks; no loops and no shared mutable state in tests.
    - Decisions: TEST-1 (placement from CFG-12).
34. **Mocks and test doubles**
    - Mocking philosophy; `mock()`, `spyOn()`, `mock.module()` and preload conventions; restoring state between tests.
    - Decisions: TEST-2.
35. **Type-level tests**
    - `expectTypeOf` coverage for exported public types. Merge candidate into topic 33 if the budget tightens.
    - Decisions: TEST-3.

### Section 7: Documentation

36. **Comments and JSDoc**
    - Why-comments over what-comments; when exported APIs get JSDoc; TODO conventions.
    - Decisions: DOC-1.

### Conditional Overlays (pending SCOPE-3)

- **React and TSX** (1-3 pages): component and props typing, hooks, ref-as-prop, context, state and data-fetching standards. Decisions: REACT-1, REACT-2.
- **CLI scripts**: Bun Shell, arg parsing, exit codes, stdout/stderr discipline.
- **Monorepo and workspaces**: workspace protocol, catalogs, per-package tsconfig and Biome nesting, project references.
- **Package publishing**: `exports` maps, `.d.ts` authoring, public type surface design.

## Workflows

Planned workflow pages (procedures, not policy), to be drafted after the core sections they depend on:

- **setup-project**: scaffold a new Bun + TS 6 + Biome project matching the resolved tsconfig/Biome baselines.
- **review-typescript**: checklist-driven review pass; the checklist derives from guideline-page bans and must be updated with them.
- **add-dependency**: vet, pin, and install a dependency (including the owner's package-age and postinstall-script rules).
- **migrate-legacy**: bring existing code up to the guide; TS 6/7 deprecations, enum and namespace removal, suppression cleanup.
