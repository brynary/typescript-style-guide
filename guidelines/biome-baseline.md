# Biome Baseline and Rule Ownership

## Rule

Use Biome 2.2 or newer as the only formatter and linter, with one canonical root config that package configs may extend: accept formatter defaults, let `.gitignore` govern file exclusion, and enable the rules that enforce this guide.

## Why

Biome owns everything a tool can decide so prose does not have to: formatting and the lint rules below are enforced automatically, leaving guideline pages to cover only the judgment calls tooling cannot make. Accepting Biome's defaults keeps the config surface small.

## Do

- Copy the canonical `biome.json` below and keep `recommended: true`.
- Install Biome 2.2 or newer and keep the `$schema` version in sync with the installed version; before 2.2, `useConsistentTypeDefinitions` and `noImportCycles` were nursery rules and this config fails to load.
- Accept the formatter defaults unchanged: tabs, double quotes, line width 80.
- Let `vcs.useIgnoreFile` exclude what `.gitignore` already ignores; add `files.includes` entries only for committed generated files.
- Enable the opinionated rules that back register decisions: `noEnum`, `noNonNullAssertion`, `noParameterAssign`, `useNamingConvention`, `useFilenamingConvention` (kebab-case), `noBarrelFile`, `noReExportAll`, `useConsistentTypeDefinitions` (interface), `useConsistentArrayType` (shorthand `T[]`), `noImportCycles`, `useThrowOnlyError`, `useThrowNewError`.
- Exempt `objectLiteralProperty` from `useNamingConvention`: object keys routinely mirror external names (HTTP headers, env vars, API fields).
- Disable `useLiteralKeys`: the tsconfig baseline's `noPropertyAccessFromIndexSignature` forces the bracket access this rule flags.
- Relax a rule for a narrow file set (test fixtures, generated bindings) with a scoped `overrides` block, never by weakening the root config.
- Run Biome in the same pre-commit and CI gates as the type check.

## Avoid

- ESLint or Prettier; Biome is the single tool.
- Overriding formatter settings (spaces, single quotes, wider lines) to match legacy style.
- Hand-maintained `files.includes` exclusion lists that duplicate `.gitignore`.
- Enabling `noExcessiveCognitiveComplexity` or other rules with no backing register decision.
- Restating a Biome-enforced rule as prose on another page; link to the owning guideline instead.

## Example

```jsonc
{
  "$schema": "https://biomejs.dev/schemas/2.2.0/schema.json",
  "vcs": { "enabled": true, "clientKind": "git", "useIgnoreFile": true },
  "formatter": {
    // Defaults left unset on purpose: tabs, lineWidth 80
    "enabled": true
  },
  "linter": {
    "enabled": true,
    "rules": {
      "recommended": true,
      "complexity": {
        // noPropertyAccessFromIndexSignature forces bracket access
        "useLiteralKeys": "off"
      },
      "style": {
        "noEnum": "error",
        "noNonNullAssertion": "error",
        "noParameterAssign": "error",
        "useNamingConvention": {
          "level": "error",
          "options": {
            "conventions": [
              // Object keys mirror external names (headers, env vars)
              { "selector": { "kind": "objectLiteralProperty" }, "match": ".*" }
            ]
          }
        },
        "useThrowOnlyError": "error",
        "useThrowNewError": "error",
        "useConsistentTypeDefinitions": {
          "level": "error",
          "options": { "style": "interface" }
        },
        "useConsistentArrayType": {
          "level": "error",
          "options": { "syntax": "shorthand" }
        },
        "useFilenamingConvention": {
          "level": "error",
          "options": { "filenameCases": ["kebab-case"] }
        }
      },
      "performance": {
        "noBarrelFile": "error",
        "noReExportAll": "error"
      },
      "suspicious": {
        "noImportCycles": "error"
      }
    }
  }
}
```

## Exceptions

- A monorepo package may add a nested `biome.json` with `"extends": "//"` to adjust rules for that package only (see [monorepo](monorepo.md)).
- A scoped relaxation stays in the root config as an `overrides` block naming the file set it covers:

```jsonc
"overrides": [
  { "includes": ["e2e/**"], "linter": { "rules": { "style": { "useFilenamingConvention": "off" } } } }
]
```
