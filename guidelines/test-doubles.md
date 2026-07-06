# Mocks and Test Doubles

## Rule

Test through real implementations or simple nullable-style fakes and mock only at process boundaries (network, clock, filesystem), restoring every mock between tests.

## Why

Real code under test catches real bugs; mocks encode assumptions that drift from the code they stand in for. Boundaries the test cannot control (I/O, time) are the narrow places a double is worth its cost, and unrestored mocks leak into later tests.

## Do

- Call the real collaborator when it is pure or cheap; inject a plain fake object that implements the same interface when it is not.
- Use `spyOn(obj, "method")` for a boundary call on an existing object, and give it a stub (`mockResolvedValue`, `mockImplementation`) so the real boundary is never hit.
- Use `mock(fn)` to build a standalone stub passed in as a dependency.
- Restore state in `afterEach` with `mock.restore()` so no double survives the test.

## Avoid

- Mocking the module or class you are actually testing.
- Reaching for `mock.module()` when a constructor argument or `spyOn` would do.
- A spy on a process boundary left calling through, so the test still hits the real network, clock, or filesystem.
- Leaving a spy or fake timer installed after the test that set it up.

## Example

```ts
import { afterEach, describe, expect, mock, spyOn, test } from "bun:test";
import { fetchLatest } from "./client.ts";

interface Clock {
  readonly now: () => number;
}

afterEach(() => {
  mock.restore();
});

describe("fetchLatest", () => {
  test("stamps the response with the injected clock", async () => {
    spyOn(globalThis, "fetch").mockResolvedValue(Response.json({ id: "a1" }));
    const clock: Clock = { now: () => 1_000 };

    const result = await fetchLatest({ id: "a1" }, clock);

    expect(result.at).toBe(1_000);
  });

  test("fetches the record exactly once", async () => {
    const fetchStub = spyOn(globalThis, "fetch").mockResolvedValue(
      Response.json({ id: "a1" }),
    );

    await fetchLatest({ id: "a1" }, { now: () => 0 });

    expect(fetchStub).toHaveBeenCalledTimes(1);
  });
});
```

## Exceptions

If `mock.module()` is genuinely required, register every module mock in one centralized preload script referenced from `bunfig.toml`, never inline per test. Test structure and lifecycle hooks are owned by [test-structure.md](test-structure.md).
