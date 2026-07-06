# Suppressions and Escape Hatches

## Rule

Every `biome-ignore` and every `@ts-expect-error` carries a written reason on the same directive, and generated, legacy, or interop code is isolated into marked zones rather than suppressed line by line.

## Why

A suppression removes a guardrail, so the reason must be visible at the point of use for the next reader. `@ts-expect-error` also fails the build once the underlying error is fixed, which prevents stale suppressions from lingering.

## Do

- Write a reason after the colon: `// biome-ignore lint/<group>/<rule>: <reason>`.
- Prefer `@ts-expect-error <reason>` over `@ts-ignore`; it surfaces when the suppression is no longer needed.
- Confine unavoidable escapes (an untyped third-party interop boundary, generated code) to a small, clearly marked module or block.
- Exclude generated directories from Biome with an `overrides`/`includes` entry so they need no per-line ignores.

## Avoid

- A bare `// biome-ignore ...` or `// @ts-ignore` with no reason.
- Suppressing a rule broadly when a one-line fix would satisfy it.
- Scattering interop suppressions through business logic instead of isolating them behind a typed wrapper.

## Example

```ts
import { legacyClient } from "./vendor/legacy-client.ts";

// Interop boundary: the vendor module ships no types.
export function fetchRawRecord(id: string): Promise<unknown> {
	// @ts-expect-error vendor/legacy-client.ts has no type declarations
	return legacyClient.get(id);
}
```

## Exceptions

- None. A suppression without a reason is not allowed anywhere.
