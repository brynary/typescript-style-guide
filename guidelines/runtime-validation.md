# Runtime Validation

## Rule

Validate every piece of external data (API responses, env vars, form input, config files) with a Zod schema at the boundary, and trust it inside.

## Why

The type system does not check values that cross a runtime boundary. A schema turns `unknown` input into a typed value once, at the edge, so the rest of the code needs no defensive checks.

## Do

- Define a Zod schema per external shape and derive the type with `z.infer<typeof Schema>`.
- Generate types from a machine-readable contract (OpenAPI, GraphQL) when one exists instead of hand-writing them; the codegen tool stays per-project.
- Parse at the boundary: `parse` when a failure is an unrecoverable startup error, `safeParse` when the caller can recover.
- Accept the input as `unknown` and let the schema narrow it.
- Turn a recoverable `safeParse` failure into a `Result` per [error-handling.md](error-handling.md).

## Avoid

- Hand-written `interface`s that duplicate an external shape a schema already describes.
- Casting external JSON to a type instead of parsing it.
- Re-validating the same value deeper in the call stack.

## Example

```ts
import { z } from "zod";

const EnvSchema = z.object({
  DATABASE_URL: z.url(),
  PORT: z.coerce.number().int().positive(),
  LOG_LEVEL: z.enum(["debug", "info", "error"]).default("info"),
});

type Env = z.infer<typeof EnvSchema>;

export function loadEnv(source: Record<string, string | undefined>): Env {
  return EnvSchema.parse(source);
}

const UserSchema = z.object({
  id: z.string(),
  email: z.email(),
});

type User = z.infer<typeof UserSchema>;

export function parseUser(payload: unknown): User | undefined {
  const result = UserSchema.safeParse(payload);
  return result.success ? result.data : undefined;
}
```

## Exceptions

Data produced and consumed entirely inside the trust boundary does not need runtime validation; a static type is enough.
