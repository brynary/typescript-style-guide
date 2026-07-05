# TypeScript Style Guide Terrain Map for AI Coding Agent Skills

## Terrain and design constraints

Your skill should be opinionated about semantics, module boundaries, and type-system usage more than low-level formatting. That is because TypeScript 6 changes several defaults in ways that now materially affect project conventions: `strict` defaults to `true`, `module` defaults to `esnext`, `target` defaults to the current supported ECMAScript year, `types` defaults to an empty array, and `noUncheckedSideEffectImports` now defaults to `true`. At the same time, the Bun docs recommend a Bun-oriented baseline with `module: "Preserve"`, `moduleResolution: "bundler"`, `verbatimModuleSyntax: true`, `allowImportingTsExtensions: true`, `noEmit: true`, `types: ["bun"]`, and several stricter flags such as `noUncheckedIndexedAccess` and `noImplicitOverride`. citeturn2view0turn3view1turn3view4turn11view0turn12view0

The skill also needs an explicit “static analysis is separate from transpilation” policy. Bun strips TypeScript syntax but does not type-check, and Vite likewise only transpiles TypeScript and recommends running `tsc --noEmit` separately during build or watch workflows. That means one of the most important pages in the eventual skill is not stylistic at all: it should tell the agent when it must run type-checking, and which `tsconfig` it should treat as authoritative. citeturn14view0turn14view1turn28search4

Because you use Biome rather than ESLint, the guide should distinguish between choices that belong in tool configuration and choices that belong in prose. Biome is intentionally opinionated and keeps formatting options relatively narrow; it can own quote style, semicolons, line width, import organization, and suppression syntax. The markdown pages should therefore spend their scarce budget mostly on conventions that Biome cannot settle by itself: interfaces versus type aliases, enum policy, nullability, public API design, unsafe escape hatches, import-topology rules, and the acceptable subset of runtime-bearing TypeScript features. citeturn13search4turn23view0turn23view1turn25view0turn24view0

A practical budget is **thirty-six core pages**, with **up to three optional overlay pages** if you want React/TSX, package publishing, and monorepo-specific guidance. That keeps you within the rough thirty-to-forty-topic ceiling while reserving space for the genuinely contentious questions. citeturn13search4turn18view1turn1search5

## Configuration and module boundaries

**Canonical `tsconfig` baseline.** One page should define the default compiler baseline the agent assumes before it writes any code. Decision points: whether to pin `target` and `lib` to fixed values or follow Bun’s `ESNext` recommendation; whether to set `rootDir` explicitly now that TypeScript 6 no longer infers it in the old way; whether `allowJs` belongs in the long-term baseline or only in migration projects. citeturn3view1turn11view0turn12view0

**Strictness profile beyond `strict`.** `strict: true` is now the default in TypeScript 6, but Bun’s sample config still leaves several stricter flags off by default. A page should settle whether the repository also standardizes `exactOptionalPropertyTypes`, `isolatedModules`, `noPropertyAccessFromIndexSignature`, `noUnusedLocals`, and `noUnusedParameters`, and whether those are global defaults or applied only in certain packages. citeturn3view4turn10search3turn15search1turn15search2turn12view0

**Type package acquisition and globals.** TypeScript 6 no longer auto-discovers all `@types/*` packages in the way earlier versions effectively did, and Bun documents that Bun projects must list `"types": ["bun"]` explicitly. The skill needs a page saying how the agent should treat `types` arrays for server, browser, test, and React packages, and whether each package should expose only the globals it truly uses. citeturn12view0turn3view1turn16search14

**Type-check workflow.** Since Bun and Vite are both transpile-first tools, the style guide should include a page on commands and phases: when to rely on editor diagnostics, when to run `tsc --noEmit`, whether watch mode is required locally, and whether `bun test` should be paired with an explicit type-check step in CI. citeturn14view0turn14view1turn1search2turn1search16

**Module kind and resolution strategy.** TypeScript 6 deprecates `moduleResolution: node` and explicitly positions `bundler` and `nodenext` as the two main forward paths. A page should decide whether your default is always Bun-or-bundler semantics, whether `nodenext` is allowed for edge packages, and whether mixed strategies are permitted inside one repository. citeturn17search4turn15search3turn15search16turn11view0

**Import and export syntax rules.** Bun recommends `verbatimModuleSyntax`, and the TypeScript docs describe it as the cleaner rule set for preserving real runtime imports while erasing type-only ones. This page should settle whether agents should always prefer named exports, whether default exports are allowed at all, and whether the skill should forbid import forms that depend on confusing interop behavior. Google’s public guide is more permissive about default imports only when outside code requires them, which is a good example of the kind of local policy this page needs to resolve. citeturn4search3turn4search18turn8view0

**Import path topology.** This page should decide whether the codebase prefers relative imports, TypeScript `paths`, or package subpath imports via `imports`. TypeScript 6 added support for `#/` subpath imports in `bundler` and `nodenext`, Node’s package `imports` field requires entries beginning with `#`, and Vite notes that resolving `tsconfig.paths` has a performance cost and cites the TypeScript team’s broader caution about using `paths` to change the behavior of external tools. citeturn3view3turn27view2turn14view1turn17search2

**Local import extensions and import attributes.** Bun explicitly supports `.ts` extensions in imports and recommends `allowImportingTsExtensions`; Bun also supports import attributes for typed loaders, while TypeScript 6 deprecates the old `asserts` syntax in favor of `with`. This page should answer whether local source imports must include extensions, may include them, or should avoid them; and it should specify the exact style to use for JSON, TOML, YAML, or asset imports. citeturn11view0turn14view0turn2view0turn4search9

**Side-effect imports and asset modules.** Because `noUncheckedSideEffectImports` is now on by default in TypeScript 6, side-effect imports deserve their own page. The page should settle whether bare imports are allowed only for specific categories like polyfills and CSS, where wildcard ambient module declarations live, and whether import organizer rules should pin bare imports at the top, bottom, or in isolated chunks. citeturn28search1turn28search2turn29search5turn25view0

**Public API surfaces and barrel files.** Node recommends the `exports` field for new packages, and that field tightly defines what consumers may import. A page should therefore decide whether agents may create barrel files freely, only at package roots, or not at all; whether internal modules may self-reference the package name; and whether public exports are curated by `package.json` rather than by convention alone. citeturn26view0turn27view2

## Type modeling decisions

**Interfaces versus type aliases.** This is a classic style-guide decision point. TypeScript’s own docs say interfaces are generally preferable when possible because they can be reopened and map naturally to object extension, and Google’s public guide also prefers interfaces over type aliases for object literals. A page should settle whether your codebase follows that rule strictly, prefers `type` unless extension/merging is needed, or uses a contextual rule. citeturn20view0turn9view2

**Enum policy.** TypeScript’s handbook explicitly warns that enums are a runtime feature you may want to avoid unless you are sure, while `const enum` is inlined and has well-documented pitfalls; `isolatedModules` also warns on features such as `const enum` and `namespace`. Google’s guide bans `const enum` but permits plain `enum`. This page needs an explicit choice among plain enums, string enums only, “enum only at boundaries,” or “prefer `as const` objects + unions everywhere.” citeturn21search6turn21search0turn21search7turn9view0

**Literal inference tools.** TypeScript now gives you three strong tools for preserving specificity without losing validation: `as const`, `satisfies`, and `const` type parameters. This page should define when agents should reach for each one, especially for config objects, discriminants, route maps, action tables, and token maps. citeturn22search15turn4search0turn22search0turn30search15

**`any`, `unknown`, and `never`.** Biome’s `noExplicitAny` rule documents `any` as a dangerous escape hatch and explicitly notes that `unknown` is often the safer alternative. TypeScript’s docs describe `unknown` as the type-safe counterpart to `any`, and `never` as the type that represents impossible values. A page should specify where `any` is outright forbidden, where `unknown` is required at trust boundaries, and how `never` is used for exhaustiveness. citeturn19view0turn4search8turn4search12turn4search20

**Assertions and non-null assertions.** TypeScript permits both `as` assertions and postfix `!`, but the handbook treats `!` as a manual override when the compiler cannot eliminate `null` or `undefined`. This page should decide whether assertions are acceptable only at runtime-validated boundaries, whether double assertions are forbidden, and whether non-null assertions require adjacent proof in code or comments. citeturn20view0turn4search8

**Absence model.** Google’s public guide says there is no universal rule that always makes `undefined` preferable to `null`; context matters, and optional fields are usually better than `| undefined` for object shapes and parameters. TypeScript’s `exactOptionalPropertyTypes` option then makes “missing” and “present but `undefined`” meaningfully different. This page should lock in one house philosophy for absence values so agents do not oscillate between styles. citeturn8view0turn10search3turn10search10

**Readonly and immutability.** TypeScript’s docs draw a clear distinction: variables use `const`, properties use `readonly`, and utility types like `Readonly` are built in. A page should decide whether DTOs, props, config objects, and public return types should default to shallow immutability, and whether mutating helpers are discouraged in favor of copy-and-return patterns. citeturn21search10turn21search15turn22search2turn22search5

**Dictionaries, index signatures, and `Record`.** `noUncheckedIndexedAccess` and `noPropertyAccessFromIndexSignature` exist because loose dictionary shapes are one of the places TypeScript can otherwise feel safer than the runtime really is. This page should say when to use `Record`, when explicit members plus a narrow index signature are acceptable, and whether unknown-key access must use bracket syntax. citeturn15search0turn15search1turn15search13

**Discriminated unions and state modeling.** TypeScript’s narrowing model strongly rewards discriminant-based unions, and `never` gives you an exact exhaustiveness hook. A page should tell agents to prefer discriminated unions over Boolean flag bags for async state, command outcomes, and reducer/action patterns, and it should settle the standard `assertNever` or exhaustiveness idiom for switches. citeturn4search1turn4search12turn20view0

**Generic design.** The TypeScript handbook warns against overusing type parameters and unnecessary constraints because they make inference worse for callers. This page should define naming conventions for type parameters, the maximum complexity agents should introduce in routine code, and when explicit type arguments are appropriate versus a smell. citeturn22search3turn32view1

**Advanced type tools.** Utility types are built in, while mapped, conditional, and template-literal types are powerful but can become hard to reason about. Google’s guide explicitly calls out maintainability and tooling downsides for mapped and conditional types. A page should decide whether these tools are encouraged in library code, discouraged in application code, or allowed only behind exported named aliases. citeturn22search2turn22search1turn22search14turn8view0

**Primitive and object-type spellings.** TypeScript’s declaration-file guidance says not to use boxed object types such as `String`, `Number`, `Boolean`, `Symbol`, or `Object`, and instead to use primitive spellings. This is simple, but it is a valuable page because agents often regress into `Object`, `{}`, or `Function` in weakly specified spots. The page should define the banned and preferred forms. citeturn4search16turn33view0

## API and implementation conventions

**Variable declarations and mutation.** The TypeScript docs recommend a least-privilege approach where bindings that are not reassigned should use `const`. A page should set the repository rule for `const`, `let`, and `var`, and whether agents should default to immutable local values plus new bindings rather than in-place mutation. citeturn21search15turn29search13

**Naming conventions.** Google’s public guide supplies a useful baseline: `UpperCamelCase` for classes, interfaces, types, enums, and type parameters; `lowerCamelCase` for variables and functions; and no `I` prefix for interfaces. A page should decide whether to follow that scheme exactly, what suffixes are allowed for DTOs or schemas, how files are named, and how tests or fixtures are named. citeturn8view0turn9view3

**Function forms and `this`.** Google prefers function declarations for named functions, arrow functions instead of function expressions, and restricted use of `this`; the TypeScript docs also show that callback APIs requiring a typed `this` must use `function`, not arrows. A page should therefore settle your default function form and list the explicit exceptions. citeturn9view1turn32view0

**Explicit return types.** This is another genuine decision point. Google leaves explicit return types as a local policy question, while Biome has a rule that can enforce them and documents both the readability/performance benefits and the corresponding verbosity cost. A page should answer whether you require explicit returns only on exported functions, on all functions, or only when inference would be unstable or non-obvious. citeturn8view0turn19view2

**Overloads versus unions versus options objects.** TypeScript documents overloads, but also explicitly says to prefer union parameters instead of overloads when possible. A page should decide when overloads are acceptable, when an options object is the preferred escape hatch for API growth, and how many overloads are too many before the agent should redesign the surface. citeturn32view0turn32view1

**Parameter objects, destructuring, and defaults.** TypeScript documents parameter destructuring clearly, and Google adds a more stylistic opinion that destructured parameters should stay simple. A page should settle when to switch from positional arguments to an object parameter, whether nested destructuring in parameters is forbidden, where defaults belong, and whether callbacks may use optional parameters in their type signatures. citeturn33view0turn33view1turn8view0

**Async APIs and error modeling.** Bun and the modern TypeScript utility types make promise-centric APIs easy, and `Awaited<T>` is built in for composition. The unresolved choice is architectural rather than syntactic: should application code prefer thrown exceptions, typed `Result` objects, discriminated union outcomes, or some hybrid depending on layer boundaries? This deserves a dedicated page because AI agents otherwise mix styles within the same codebase. citeturn22search2turn14view0

**Narrowing and guard helpers.** TypeScript’s narrowing support includes `in`, `typeof`, `instanceof`, user-defined predicates, and control-flow analysis. A page should specify whether the codebase likes small reusable guard helpers, where `asserts value is T` helpers are acceptable, and when to refactor deeply nested narrowing into separate functions for clarity. citeturn4search1turn20view0

**Comments, JSDoc, and TSDoc-like expectations.** Google’s style guide includes explicit expectations for file-level comments, Markdown in JSDoc, and descriptive class comments. A page should decide whether comments are mostly for exported APIs, whether internal code prefers self-documenting names over prose, and what block-level docs agents should add for tricky conditional types, invariants, or unsafe interop. citeturn8view0

**Generated, legacy, and interop boundaries.** Some code will inevitably violate the preferred subset: generated files, migration shims, framework patches, or runtime interop layers. This page should define where the guide relaxes, how those zones are marked, and whether agents should isolate them into clearly named modules instead of spreading one-off exceptions. Biome’s override system and suppression comments make that separation enforceable. citeturn18view1turn24view0

## Runtime and tooling policies

**Class-first or function-first design.** TypeScript fully supports classes, but much application code can be written more simply with functions, objects, and plain data structures. A page should decide whether agents should default to function/object composition and reserve classes for framework integration, stateful abstractions, and domain types that benefit from methods and inheritance. citeturn4search15turn21search14

**Class fields, visibility, and runtime semantics.** This page should settle `public`/`private`/`protected` style, use of `readonly`, constructor parameter properties, and whether `#private` is allowed. This is especially important because `useDefineForClassFields` changes runtime behavior toward the ECMAScript standard, and TypeScript’s class docs explain how `declare` can avoid unintended runtime field initialization in subclasses. Google’s public guide also explicitly rejects `#private` in favor of TypeScript visibility annotations, which shows this remains a living style choice rather than settled doctrine. citeturn16search0turn16search10turn16search16turn9view3

**Inheritance, composition, and `override`.** Bun’s recommended config enables `noImplicitOverride`, and the TypeScript docs explain why `override` protects subclasses from drifting when base APIs change. A page should therefore define when inheritance is acceptable, when abstract classes beat interfaces, and whether agents should default to composition unless they are intentionally extending a stable framework contract. citeturn12view0turn16search1turn16search9

**Decorators policy.** TypeScript now supports modern decorators, but `experimentalDecorators` still exists for the older pre-standard model, and Biome’s `useImportType` docs call out a real caveat for frameworks that depend on decorator-driven runtime metadata. This page should decide whether decorators are allowed at all, whether only standard decorators are allowed, whether metadata-emitting legacy decorators are confined to framework packages, and whether `useImportType` is relaxed in those areas. citeturn30search1turn30search6turn19view1

**Runtime-bearing TypeScript features and the erasable subset.** TypeScript 5.8 introduced `erasableSyntaxOnly`, and the docs explain that it rejects many TS-only constructs with runtime behavior. Separately, `isolatedModules` warns on constructs such as namespaces and `const enum`, and TypeScript 6 deprecates the legacy `module` syntax for namespaces. A page should define the acceptable runtime-bearing subset for your repository: native enums or not, parameter properties or not, namespaces never, legacy decorator modes where necessary, and whether the codebase aims to stay as close as possible to erasable TypeScript. citeturn10search4turn10search1turn15search2turn2view0

**Biome ownership, suppressions, and rule mapping.** One page should define what the skill assumes Biome will enforce. Biome can organize imports, enforce `import type`, flag explicit `any`, remove unused imports, and support inline, file-wide, and range suppressions with required explanations. This page should settle which Biome rules are “hard law,” which are advisory, when suppressions are acceptable, and whether every suppression requires a reason and often a follow-up issue or TODO. citeturn19view0turn19view1turn19view3turn24view0turn25view0

**Testing and type tests.** Bun’s built-in test runner supports TypeScript directly and also supports `expectTypeOf`, which is valuable for locking down public types and complex inference behavior. A page should define test file naming, when to add type tests versus runtime tests, and whether exported utility types, discriminated unions, and overloads must have dedicated type-level coverage. citeturn1search2turn1search10turn1search16

**Optional React and TSX overlay.** If some packages use React with Vite for HMR, add a dedicated overlay page rather than polluting the core TypeScript guide. Bun supports React and `.tsx`, while Vite provides fast HMR and transpiles TypeScript without type checking. That page can decide props typing patterns, `children` conventions, component export style, hook return typing, and whether component-local helper types live inline or in adjacent files. citeturn6search3turn14view1turn0search15

**Optional package-publishing and declaration overlay.** If any workspace publishes packages, add a page on package `exports`, `.d.ts` authoring, module augmentation, and public type surface design. Node’s package docs strongly emphasize `exports` for new packages, and the TypeScript declaration-file docs emphasize matching declaration layout to library layout and handling augmentations deliberately. citeturn26view0turn29search10turn29search0turn29search7

**Optional monorepo overlay.** If you have many packages, a dedicated page should define workspace dependency rules, config inheritance, and package-boundary imports. Bun workspaces support filtered installs and workspace protocols, while Biome v2 supports `extends`, nested configs, overrides, and monorepo-specific root handling. citeturn1search5turn1search12turn18view1

## Decision points that should be settled first

The terrain above is broad, but a small set of decisions will determine the rest of the skill. In practice, I would lock these before drafting any markdown pages, because they cascade into many downstream examples and rules. The underlying pressure comes from TypeScript 6’s defaults, Bun’s recommended config, Vite’s transpile-only model, and Biome’s rule surface. citeturn3view4turn12view0turn14view1turn19view1

| Decision to lock early | Options you need to choose between |
|---|---|
| Module baseline | `preserve + bundler` everywhere, or allow `nodenext` islands |
| Import path strategy | relative imports, `tsconfig.paths`, or package `#/` subpath imports |
| Local import extension style | always extensionless, always extensioned, or contextual |
| Default export policy | named exports only, default exports allowed selectively, or default-friendly |
| Interface vs type alias | interface-first, type-first, or contextual |
| Enum policy | plain enums allowed, string enums only, or enums banned in favor of `as const` objects |
| Explicit return types | exported-only, all functions, or only complex/public APIs |
| `any` policy | prohibited, boundary-only, or migration-only with comment |
| Nullability policy | `undefined`-first, `null`-first at boundaries, or mixed by context |
| Assertion policy | `as`/`!` only at validated boundaries, broadly allowed, or helper-only |
| Class policy | function-first, class-friendly, or framework-specific |
| Private field policy | `private`, `#private`, or both with rules |
| Decorator policy | no decorators, standard decorators only, or legacy framework carve-outs |
| Strictness add-ons | whether to enable `exactOptionalPropertyTypes`, `isolatedModules`, `noUnused*`, and `noPropertyAccessFromIndexSignature` |
| Barrel-file policy | forbidden, package-root-only, or broadly allowed |
| Testing policy | runtime tests only, plus type tests for public surfaces, or type tests only for libraries |
| Exception handling | Biome suppressions with required explanations, config overrides, or isolated boundary folders |

If you want the skill to stay under the top of your budget, the best packaging shape is:

- **Thirty-six core pages** covering the cross-repository rules.
- **Up to three optional overlay pages** for React/TSX, package publishing, and monorepo conventions.
- **Very little page budget spent on formatting**, because Biome can already encode most of that mechanically. citeturn13search4turn23view0turn23view1turn18view1

That structure gives you a skill that is narrow enough for agents to consult quickly, but broad enough to settle the choices that actually change emitted TypeScript, runtime behavior, and long-term maintainability. citeturn2view0turn12view0turn14view0turn14view1