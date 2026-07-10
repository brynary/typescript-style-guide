# Module Resolution Baseline

## Rule

Write ESM only and resolve modules with `module: "preserve"` plus `moduleResolution: "bundler"` everywhere, reserving a `nodenext` island only for a package that ships to Node consumers.

## Why

`preserve` + `bundler` matches how Bun and Vite actually run and bundle the code, so imports resolve the same way at type-check time and at runtime. `nodenext` only earns its stricter, extension-sensitive rules when a package must be consumed by plain Node.

## Do

- Author every module as ESM: `import` / `export`, top-level `await`, no `require`.
- Keep `module: "preserve"` and `moduleResolution: "bundler"` in the shared tsconfig.
- Isolate a Node-facing published package in its own tsconfig set to `module: "nodenext"` when it must run under Node without a bundler.

## Avoid

- `require`, `module.exports`, or `__dirname`/`__filename` (CommonJS).
- Switching the whole repo to `nodenext` because one package ships to Node.
- Mixing resolution modes inside a single package.

## Example

```jsonc
// tsconfig.json (shared baseline)
{
  "compilerOptions": {
    "module": "preserve",
    "moduleResolution": "bundler"
  }
}
```

```ts
// ESM only: static imports and named exports
import { hostname } from "node:os";

export function currentHost(): string {
  return hostname();
}
```

## Exceptions

A package published for Node consumers may use a `nodenext` island in its own tsconfig; it then follows Node's extension and `exports` rules. See [tsconfig-baseline.md](tsconfig-baseline.md) for the full compiler config and [monorepo.md](monorepo.md) for per-package tsconfig nesting.
