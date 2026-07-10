# CLI Scripts

Conventions for Bun-executed command-line scripts and automation.

## Activation

Apply when writing a runnable script, automation task, or CLI entry point. Skip for library or application module code.

## Rule

Write scripts as Bun programs that shell out with Bun Shell (`$`), parse arguments with `util.parseArgs`, and signal failure by setting `process.exitCode` rather than exiting mid-flight.

## Why

Bun Shell runs the same Unix-like syntax on every platform and escapes interpolated values by default. `parseArgs` is built in, so no dependency is needed. Setting `process.exitCode` lets pending output and `finally` cleanup complete before the process ends.

## Do

- Start executable scripts with `#!/usr/bin/env bun` and `chmod +x` them.
- Import the shell as `import { $ } from "bun"` and interpolate values with `${}` so Bun escapes them.
- Parse flags with `parseArgs` over `Bun.argv.slice(2)`.
- Write program output to stdout (`console.log`, `Bun.write`) and diagnostics, progress, and errors to stderr (`console.error`).
- Set `process.exitCode = 1` on failure; reserve `process.exit()` for cases where no cleanup remains.

## Avoid

- Building shell command strings by concatenation (injection risk).
- `process.exit()` when writes or disposables still need to flush.
- Mixing diagnostics into stdout that a caller may pipe or parse.

## Example

```ts
#!/usr/bin/env bun
import { $ } from "bun";
import { parseArgs } from "node:util";

const { values } = parseArgs({
  args: Bun.argv.slice(2),
  options: {
    branch: { type: "string" },
  },
});

const branch = values.branch ?? "main";

try {
  await $`git fetch origin ${branch}`;
  console.log(`fetched ${branch}`);
} catch {
  console.error(`failed to fetch ${branch}`);
  process.exitCode = 1;
}
```

## Exceptions

A script published for Node consumers may need a Node-portable shebang and APIs; see [bun-runtime-and-dependencies.md](bun-runtime-and-dependencies.md).
