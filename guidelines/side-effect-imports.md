# Side-Effect Imports and Asset Modules

## Rule

Keep `noUncheckedSideEffectImports` on and make asset imports type-check by declaring wildcard ambient modules for CSS, SVG, and other assets in a single global `.d.ts`.

## Why

With the check on, a bare side-effect import that resolves to nothing is an error, which catches typos and dead imports. Wildcard ambient declarations tell TypeScript what an asset import produces without loosening the check for the whole project.

## Do

- Use a bare `import "./styles.css"` only for real side effects such as stylesheets or polyfills.
- Declare each asset kind once in a global declaration file (for example `src/assets.d.ts`).
- Give an asset module the shape the loader actually provides (a URL string, a component, etc.).

## Avoid

- Setting `noUncheckedSideEffectImports: false` to silence unresolved imports.
- Scattering `declare module` blocks across feature files.
- Using a side-effect import to pull in values that should be named imports.

## Example

```ts
// src/assets.d.ts
declare module "*.css";

declare module "*.svg" {
  const src: string;
  export default src;
}
```

```ts
import "./theme.css";
import iconSrc from "./icon.svg";

export function iconTag(): string {
  return `<img src="${iconSrc}" alt="" />`;
}
```

## Exceptions

An asset module's default export is the loader's contract, not a violation of named-export style; see [export-style.md](export-style.md). Keeping the check enabled is part of the compiler baseline in [tsconfig-baseline.md](tsconfig-baseline.md).
