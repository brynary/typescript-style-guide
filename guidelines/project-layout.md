# Project Layout and File Naming

## Rule

Put source under `src/` organized into layer folders (`routes/`, `services/`, `models/`), colocate each test as `foo.test.ts` beside the file it covers, and name every file in kebab-case.

## Why

Layer folders group code by technical role so an agent knows where a route, service, or model belongs without scanning the tree. Colocated tests keep a unit and its test together, and kebab-case names are enforceable and case-safe across filesystems.

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
