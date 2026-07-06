# Biome Baseline and Rule Ownership

## Rule

Use Biome 2.x as the only formatter and linter with one canonical `biome.json`: accept every formatter default and enable the opinionated rules that mechanically enforce this guide's decisions.

## Why

Biome owns everything a tool can decide so prose does not have to: formatting and the lint rules below are enforced automatically, leaving guideline pages to cover only the judgment calls tooling cannot make. Accepting Biome's defaults keeps the config surface small.

## Do

- Copy the canonical `biome.json` below and keep `recommended: true`.
- Accept the formatter defaults unchanged: tabs, double quotes, line width 80.
- Enable the opinionated rules that back register decisions: `noEnum`, `noNonNullAssertion`, `noParameterAssign`, `useNamingConvention`, `useFilenamingConvention` (kebab-case), `noBarrelFile`, `noReExportAll`, `useConsistentTypeDefinitions` (interface), `useConsistentArrayType` (shorthand `T[]`), `noImportCycles`.
- Run Biome in the same pre-commit and CI gates as the type check.

## Avoid

- ESLint or Prettier; Biome is the single tool.
- Overriding formatter settings (spaces, single quotes, wider lines) to match legacy style.
- Enabling `noExcessiveCognitiveComplexity` or other rules with no backing register decision.
- Restating a Biome-enforced rule as prose on another page; link to the owning guideline instead.

## Example

```jsonc
{
  "$schema": "https://biomejs.dev/schemas/2.1.4/schema.json",
  "formatter": {
    // Defaults left unset on purpose: tabs, lineWidth 80
    "enabled": true
  },
  "linter": {
    "enabled": true,
    "rules": {
      "recommended": true,
      "style": {
        "noEnum": "error",
        "noNonNullAssertion": "error",
        "noParameterAssign": "error",
        "useNamingConvention": "error",
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
