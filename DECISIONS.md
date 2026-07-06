# Decision Register

All rows were resolved with the project owner on 2026-07-06. Resolved decisions are policy for drafted pages. New policy questions should be added here before changing policy pages.

IDs group by area: SCOPE (guide posture), CFG (toolchain and configuration), MOD (modules and imports), TYPE (type modeling), FUN (functions and classes), ERR (errors, async, data), TEST (testing), DOC (documentation), REACT (conditional overlay). Flagship rows gate multiple pages; they are marked with an asterisk on the ID.

Sources: the three research reports in `.ai/research/`. "Consensus" marks options the reports identify as community consensus or strong 2026-era trend. "Owner rule" marks options already fixed by the project owner's standing agent instructions. Suggested defaults are kept as context for future policy changes, not as separate authority.

## Scope and Posture

| ID | Decision | Options | Suggested default | Resolution |
| --- | --- | --- | --- | --- |
| SCOPE-1* | Overall prescriptiveness | (a) hard rules everywhere; (b) hard rules for mechanical choices, reasoned defaults for judgment calls; (c) advisory | (b) - all three reports recommend this split | (b) |
| SCOPE-2 | Core page budget | (a) ~30 pages; (b) ~36 core pages plus conditional overlays; (c) up to 40 | (b) - matches the outline's guideline map | (b) |
| SCOPE-3* | Overlays in scope | any of: React/TSX, CLI scripts, monorepo/workspaces, package publishing | decide from the target codebases; reports treat all four as overlays, not core | React/TSX, CLI scripts, and monorepo/workspaces are in scope; package publishing is out |
| SCOPE-4 | Version baseline | (a) TS 6 now, written TS 7-forward (erasable-first, no deprecated options); (b) TS 6 only | (a) - TS 7 turns TS 6 deprecations into hard errors | (a) |

## Toolchain and Configuration

| ID | Decision | Options | Suggested default | Resolution |
| --- | --- | --- | --- | --- |
| CFG-1* | Strictness beyond `strict` | which of `noUncheckedIndexedAccess`, `exactOptionalPropertyTypes`, `noPropertyAccessFromIndexSignature`, `noImplicitReturns`, `noImplicitOverride`, `noUnusedLocals`/`Parameters` are mandatory | enable all; `exactOptionalPropertyTypes` and `noUncheckedIndexedAccess` add real friction, so confirm those two explicitly | Enable all, including `noUncheckedIndexedAccess` and `exactOptionalPropertyTypes` (confirmed explicitly) |
| CFG-2* | `erasableSyntaxOnly` | (a) on globally; (b) off, rely on build-step transpilation | (a) - consensus direction for Bun type-stripping and TS 7; cascades: bans enums, namespaces, parameter properties, legacy decorators | (a) |
| CFG-3 | `target` and `module` pinning | (a) follow TS 6 floating defaults (`es2025`, `esnext`); (b) pin fixed versions | (a) | (a) |
| CFG-4 | Type-check gate (Bun and Vite do not type check) | (a) `tsc --noEmit` pre-commit and in CI; (b) CI only; (c) editor diagnostics only | (a) | (a) |
| CFG-5 | `types` array policy | (a) explicit per package (e.g. `["bun"]`); (b) restore wildcard discovery | (a) - TS 6 default; consensus | (a) |
| CFG-6* | Biome formatter settings | (a) accept defaults (tabs, double quotes, lineWidth 80); (b) override (spaces/single/100-120 common) | (a) - report recommendation: smallest config surface | (a) |
| CFG-7* | Opinionated Biome rules to enable (off by default) | choose among `noEnum`, `noNonNullAssertion`, `noParameterAssign`, `useNamingConvention`, `useFilenamingConvention`, `noBarrelFile`, `noReExportAll`, `useConsistentTypeDefinitions`, `useConsistentArrayType`, `noExcessiveCognitiveComplexity` | enable the set that mechanically enforces the resolved TYPE/MOD/FUN rows | Enable `noEnum`, `noNonNullAssertion`, `noParameterAssign`, `useNamingConvention`, `useFilenamingConvention` (kebab-case), `noBarrelFile`, `noReExportAll`, `useConsistentTypeDefinitions` (interface), `useConsistentArrayType` (shorthand `T[]`); leave `noExcessiveCognitiveComplexity` off (no register row backs it) |
| CFG-8 | Suppression policy | (a) every `biome-ignore` / `@ts-expect-error` carries a written reason; (b) free-form | (a) | (a) |
| CFG-9 | Bun-first vs Node-portable APIs | (a) prefer Bun APIs (`Bun.file`, `Bun.serve`, Bun Shell); (b) stay Node-portable | (a) - owner rule: prefer Bun | (a) |
| CFG-10 | Dependency versioning | (a) exact pins plus workspace catalogs for shared versions; (b) caret ranges | (a) | (a) |
| CFG-11 | Folder organization | (a) feature folders; (b) layer folders | contested; no report consensus | (b) - owner choice |
| CFG-12 | Test file placement | (a) colocated `foo.test.ts`; (b) separate `tests/` tree | (a) - common community convention | (a) |
| CFG-13 | File naming case | (a) kebab-case; (b) camelCase; (c) match main export | (a) - enforceable via `useFilenamingConvention` | (a) |

## Modules and Imports

| ID | Decision | Options | Suggested default | Resolution |
| --- | --- | --- | --- | --- |
| MOD-1 | Module resolution baseline | (a) `module: preserve` + `moduleResolution: bundler` everywhere; (b) allow `nodenext` islands for published packages | (a), with (b) only where packages ship to Node consumers; ESM only (owner rule) | (a), with `nodenext` islands only for packages shipped to Node consumers; ESM only |
| MOD-2 | Type import style | (a) top-level `import type { X }`; (b) inline `import { type X }` | (a) - pairs with `verbatimModuleSyntax` and Biome `useImportType` | (a) |
| MOD-3* | Export style | (a) named exports only; (b) default exports allowed where a framework requires them | (a) - consensus for agent greppability; frameworks as narrow exception | (a), with defaults only where a framework mechanically requires them |
| MOD-4 | Import path strategy | (a) relative imports; (b) `package.json` `#` subpath imports; (c) `tsconfig` `paths` | (a) locally, (b) across areas; avoid (c) - runtime-ignored and a known Bun monorepo gotcha | (a) locally, (b) across areas; (c) banned |
| MOD-5 | Local import extensions | (a) always `.ts` (`allowImportingTsExtensions`); (b) extensionless | contested; Bun docs recommend (a) | (a) |
| MOD-6* | Barrel files | (a) banned; public surface curated by `package.json` `exports`; (b) package entrypoints only; (c) allowed | (a) - barrels hurt tree-shaking and invite cycles (owner rule: no circular dependencies) | (a) |
| MOD-7 | Side-effect imports | (a) keep `noUncheckedSideEffectImports` (TS 6 default), declare asset wildcards in a global `.d.ts`; (b) disable the check | (a) | (a) |

## Type Modeling

| ID | Decision | Options | Suggested default | Resolution |
| --- | --- | --- | --- | --- |
| TYPE-1* | `interface` vs `type` | (a) `interface` for object shapes, `type` otherwise; (b) `type` everywhere | contested; TS docs and Google prefer (a); Biome `useConsistentTypeDefinitions` can enforce either - pick exactly one | (a) |
| TYPE-2* | Enum policy | (a) ban `enum` and `const enum`; use `as const` objects or literal unions; (b) string enums only; (c) plain enums allowed | (a) - strong consensus; also cascades from CFG-2 | (a) |
| TYPE-3* | `any` policy | (a) banned in production code; `unknown` + narrowing at boundaries; (b) warn only | (a) - owner rule; Biome `noExplicitAny` | (a) |
| TYPE-4* | Assertions and non-null | (a) ban `as` and `!` in production code except `as const`; prefer `satisfies`; (b) allowed at validated boundaries | (a) - owner rule; Biome `noNonNullAssertion` | (a) |
| TYPE-5* | Absence model | (a) `undefined`-first; optional properties over `\| undefined`; `null` only at external boundaries; (b) mixed by context | (a) - TS-idiomatic; interacts with `exactOptionalPropertyTypes` (CFG-1) | (a) |
| TYPE-6 | Immutability defaults | (a) `readonly` by default for DTOs, props, and public return types; (b) opt-in `readonly` | (a), weakly held | (a) |
| TYPE-7 | Discriminant field name | (a) `type`; (b) `kind`; (c) `_tag` | (a) most common; if a Result library is adopted (ERR-1), follow its convention | (a) for our own unions; neverthrow's own conventions apply to Result values |
| TYPE-8 | Type-level complexity ceiling | (a) conservative in app code; mapped/conditional/template-literal types only behind named exported aliases in library code; (b) unrestricted | (a) - explicitness over cleverness for agent-maintained code | (a) |

## Functions and Classes

| ID | Decision | Options | Suggested default | Resolution |
| --- | --- | --- | --- | --- |
| FUN-1 | Naming scheme | (a) Google baseline: `lowerCamelCase` values/functions, `UpperCamelCase` types, no `I` prefix, `CONSTANT_CASE` module constants; (b) custom scheme | (a) - consensus | (a) |
| FUN-2 | Function form | (a) `function` declarations top-level, arrows for callbacks; (b) arrow consts everywhere | (a) - greppability and hoisting; genuinely contested | (a) - owner choice |
| FUN-3* | Explicit return types | (a) required on exported functions; (b) required on all functions; (c) inferred everywhere | (a) | (a) |
| FUN-4 | Class policy | (a) function-first; classes for stateful abstractions and framework contracts; (b) class-friendly | (a) | (a) |
| FUN-5 | Private member style | (a) TS `private` keyword; (b) `#private` fields | contested; Google prefers (a), (b) is runtime-enforced ECMAScript | (a) - owner choice |
| FUN-6 | Decorators | (a) none; (b) standard TC39 decorators only; (c) legacy carve-outs for framework packages | (a) unless a framework in scope requires them | (a) |

## Errors, Async, and Data

| ID | Decision | Options | Suggested default | Resolution |
| --- | --- | --- | --- | --- |
| ERR-1* | Error model | (a) exceptions with typed error classes; (b) Result types (neverthrow); (c) hybrid: Result for expected failures, throw for bugs | contested in all reports; owner tooling includes a neverthrow skill, which suggests (b) or (c) | (c) - neverthrow `Result` for expected/recoverable failures; `throw` only for bugs and unrecoverable states |
| ERR-2* | Validation library | (a) Zod; (b) Valibot; (c) ArkType; (d) none | (a) - ecosystem default per research; (b) if frontend bundle size is critical | (a) |
| ERR-3 | Date and time | (a) Temporal for new code, `Date` banned; (b) coexistence | (a) for new code, `Date` allowed at library boundaries | (a) for new code; `Date` allowed only at library boundaries |
| ERR-4 | Resource cleanup | (a) prefer `using` / `await using` for disposables; (b) `try`/`finally` acceptable | (a) preferred, (b) permitted | (a) preferred, (b) permitted |
| ERR-5 | Modern stdlib adoption | (a) prefer native `Promise.try`, `Map.getOrInsert`, `RegExp.escape`; (b) allow legacy patterns | (a) | (a) |

## Testing

| ID | Decision | Options | Suggested default | Resolution |
| --- | --- | --- | --- | --- |
| TEST-1 | Test naming and style | (a) `test` + `describe`, `*.test.ts`; (b) `it`-style BDD | (a) - matches bun:test docs | (a) |
| TEST-2* | Mocking philosophy | (a) minimal mocks; test through real implementations or nullables, mock only at process boundaries; (b) liberal `mock.module` with centralized preload; (c) inline module mocks per file | (a) - owner tooling includes a testing-without-mocks skill; if module mocks are used, centralize them in a preload script | (a) |
| TEST-3 | Type-level tests | (a) `expectTypeOf` for exported public types of library packages; (b) none | (a) for libraries only | (a) |

## Documentation

| ID | Decision | Options | Suggested default | Resolution |
| --- | --- | --- | --- | --- |
| DOC-1 | Comment and JSDoc policy | (a) JSDoc required on all exported symbols; (b) minimal: why-comments only, JSDoc where an exported API is non-obvious | (b) - matches owner's low-noise comment preference | (b) |

## React Overlay (SCOPE-3: in scope)

| ID | Decision | Options | Suggested default | Resolution |
| --- | --- | --- | --- | --- |
| REACT-1 | Component typing | (a) plain function with typed props; (b) `React.FC` | (a) - community consensus; props type keyword follows TYPE-1 | (a) |
| REACT-2 | React 19 idioms and libraries | ref-as-prop vs `forwardRef`; state library standard; data-fetching standard; styling approach | ref-as-prop (React 19); remaining choices need owner input | ref-as-prop; Tailwind CSS is the styling standard; state management and data fetching stay per-project (no prescribed library) |
