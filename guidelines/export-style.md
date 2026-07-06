# Export Style

## Rule

Use named exports for everything; add a default export only where a framework mechanically requires one.

## Why

Named exports are greppable, refactor-safely renamed, and let a bundler tree-shake unused code. Default exports hide the real symbol name and invite inconsistent import names across the codebase.

## Do

- Export every function, value, and type by name.
- Re-import a symbol under its original name.
- Give each module a clear, discoverable public surface via its named exports.

## Avoid

- `export default` in ordinary application or library modules.
- `export default { ... }` object bags that group unrelated helpers.
- Renaming a symbol at the import site to work around a default export.

## Example

```ts
export interface Money {
  readonly amount: number;
  readonly currency: string;
}

export function formatMoney(value: Money): string {
  return `${value.amount} ${value.currency}`;
}
```

## Exceptions

Use a default export only when a framework's convention requires it mechanically, such as a file-based route module or a bundler-recognized config file. Keep the exception to that single required symbol and use named exports for everything else in the file. See [react-components.md](react-components.md) for framework component conventions.
