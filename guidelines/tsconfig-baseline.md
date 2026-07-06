# tsconfig Baseline

## Rule

Use one canonical strict `tsconfig.json` for TypeScript 6: `strict` plus every strictness add-on, `erasableSyntaxOnly` and `verbatimModuleSyntax` on, and `noEmit` because Bun runs TypeScript directly.

## Why

TypeScript 6 is strict by default but leaves the sharper add-ons off; turning them all on catches real bugs before runtime. `erasableSyntaxOnly` keeps every file runnable by Bun's type-stripping loader, and this file is the single source of truth every later page defers to for compiler mechanics.

## Do

- Copy the canonical config below verbatim into a new project's root `tsconfig.json`.
- Enable every add-on: `noUncheckedIndexedAccess`, `exactOptionalPropertyTypes`, `noPropertyAccessFromIndexSignature`, `noImplicitReturns`, `noImplicitOverride`, `noUnusedLocals`, `noUnusedParameters`.
- Keep `erasableSyntaxOnly: true`; this bans enums, namespaces, and constructor parameter properties across the project.
- Set `noEmit: true`; Bun executes source, so `tsc` only type-checks (see [type-check-workflow](type-check-workflow.md)).
- Write TS 7-forward: omit deprecated options and any flag TS 7 removes.

## Avoid

- Relaxing a strictness flag to make existing code compile; fix the code or suppress narrowly (see [suppressions](suppressions.md)).
- Pinning `target` or `lib` to a fixed ECMAScript year; let the TS 6 floating defaults (`es2025`) apply.
- Setting `types` to wildcard discovery (see [type-packages](type-packages.md)) or emitting build output.
- Module and resolution settings beyond the canonical values; their policy lives in [module-resolution](module-resolution.md).

## Example

```jsonc
{
  "compilerOptions": {
    // Modules: Bun/bundler semantics (policy: module-resolution.md)
    "module": "preserve",
    "moduleResolution": "bundler",
    "verbatimModuleSyntax": true,
    "allowImportingTsExtensions": true,
    "resolveJsonModule": true,

    // TS 6 default, kept explicit (policy: side-effect-imports.md)
    "noUncheckedSideEffectImports": true,

    // Bun runs TS directly; type-check only
    "noEmit": true,
    "isolatedModules": true,

    // TS 7-forward: annotations must be fully erasable
    "erasableSyntaxOnly": true,

    // Type packages are explicit (policy: type-packages.md)
    "types": ["bun"],

    // strict plus every add-on
    "strict": true,
    "noUncheckedIndexedAccess": true,
    "exactOptionalPropertyTypes": true,
    "noPropertyAccessFromIndexSignature": true,
    "noImplicitReturns": true,
    "noImplicitOverride": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,

    "skipLibCheck": true
  }
}
```

## Exceptions

- A frontend package adds `"dom"` and `"dom.iterable"` to `lib` and `"jsx": "react-jsx"`; see [react-components](react-components.md).
- A package published to Node consumers may use a `nodenext` island (module-resolution.md), which changes `module` and `moduleResolution` only for that package.
