# Type Packages and Globals

## Rule

List every global type package explicitly in the `types` array; never restore wildcard `@types` discovery.

## Why

TypeScript 6 defaults `types` to an empty array, so no `@types/*` package is loaded unless named. An explicit list makes a file's ambient globals obvious and stops an unrelated transitive `@types` dependency from silently changing the global scope.

## Do

- Set `types` to exactly the global-augmenting packages the project uses, e.g. `["bun"]`.
- Add each runtime's globals by name: a frontend adds `"react"`; a package targeting Node adds `"node"`.
- Keep the array in the root `tsconfig.json` (see [tsconfig-baseline](tsconfig-baseline.md)); override it per package only when a package needs different globals.

## Avoid

- Removing the `types` key to re-enable automatic `@types` discovery.
- Adding a `@types/*` package that a dependency already pulls in transitively just to get its globals; name it in `types` instead of relying on discovery.
- Listing packages that do not define globals; a normally imported library does not belong in `types`.

## Example

```jsonc
{
  "compilerOptions": {
    // Only these packages contribute ambient globals
    "types": ["bun"]
  }
}
```

A frontend package extends the list rather than opening discovery:

```jsonc
{
  "compilerOptions": {
    "types": ["bun", "react"]
  }
}
```

## Exceptions

- A package that runs on Node instead of Bun replaces `"bun"` with `"node"` in its own `types` array.
