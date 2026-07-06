# Project Layout and File Naming

## Rule

Put source under `src/` organized into layer folders (`routes/`, `services/`, `models/`), colocate each test as `foo.test.ts` beside the file it covers, and name every file in kebab-case.

## Why

Layer folders group code by technical role so an agent knows where a route, service, or model belongs without scanning the tree. Colocated tests keep a unit and its test together, and kebab-case names are enforceable and case-safe across filesystems.

## Do

- Keep application code in `src/`, split by layer: `src/routes/`, `src/services/`, `src/models/`.
- Name a test after its subject with a `.test.ts` suffix, in the same folder: `user-service.ts` and `user-service.test.ts`.
- Name every file kebab-case, enforced by Biome's `useFilenamingConvention` (see [biome-baseline](biome-baseline.md)).

## Avoid

- Feature folders that mix routes, services, and models per feature.
- A separate top-level `tests/` tree mirroring `src/`.
- camelCase, PascalCase, or snake_case file names.

## Example

```
src/
  routes/
    health.ts
    health.test.ts
  services/
    user-service.ts
    user-service.test.ts
  models/
    user.ts
    user.test.ts
```

## Exceptions

- A React component file follows the same kebab-case rule (`user-card.tsx`); the exported component name stays PascalCase (see [react-components](react-components.md)).
