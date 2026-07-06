# Mocks and Test Doubles

## Rule

Test through real implementations or simple nullable-style fakes and mock only at process boundaries (network, clock, filesystem), restoring every mock between tests.

## Why

Real code under test catches real bugs; mocks encode assumptions that drift from the code they stand in for. Boundaries the test cannot control (I/O, time) are the narrow places a double is worth its cost, and unrestored mocks leak into later tests.

## Do

- Call the real collaborator when it is pure or cheap; inject a plain fake object that implements the same interface when it is not.
- Use `spyOn(obj, "method")` to observe or stub a boundary call on an existing object.
- Use `mock(fn)` to build a standalone stub passed in as a dependency.
- Restore state in `afterEach` with `mock.restore()` so no double survives the test.

## Avoid

- Mocking the module or class you are actually testing.
- Reaching for `mock.module()` when a constructor argument or `spyOn` would do.
- Leaving a spy or fake timer installed after the test that set it up.

## Example

```ts
import { describe, test, expect, spyOn, afterEach, mock } from "bun:test";
import { fetchLatest } from "./client.ts";

interface Clock {
  readonly now: () => number;
}

afterEach(() => {
  mock.restore();
});

describe("fetchLatest", () => {
  test("stamps the response with the injected clock", () => {
    const clock: Clock = { now: () => 1_000 };
    const result = fetchLatest({ id: "a1" }, clock);
    expect(result.at).toBe(1_000);
  });

  test("reads the response body once", () => {
    const read = spyOn(globalThis, "fetch");
    fetchLatest({ id: "a1" }, { now: () => 0 });
    expect(read).toHaveBeenCalledTimes(1);
  });
});
```

## Exceptions

If `mock.module()` is genuinely required, register every module mock in one centralized preload script referenced from `bunfig.toml`, never inline per test. Test structure and lifecycle hooks are owned by [test-structure.md](test-structure.md).
