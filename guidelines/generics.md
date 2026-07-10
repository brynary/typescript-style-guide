# Generics

## Rule

Introduce a type parameter only when a function or type genuinely relates its inputs and outputs, name parameters descriptively, and keep mapped, conditional, and template-literal types out of application code, allowing them only behind a named exported alias in library code.

## Why

Generics that thread a real relationship remove duplication and assertions; generics added speculatively add noise. Advanced type-level machinery is hard for agents to read and maintain, so it must be named, exported, and confined to library code where it earns its place.

## Do

- Add a type parameter when the return type depends on an argument's type.
- Name parameters for their role: `T` for a single generic, `Key`, `Value`, `Item`.
- Constrain parameters (`<Key extends string>`) instead of leaving them open.
- If a mapped or conditional type is warranted, define it as one named exported alias.

## Avoid

- A type parameter used in only one position (it should be a plain type).
- Single-letter names beyond an obvious `T`, such as `A`, `B`, `Z`.
- Inline conditional or template-literal types in application code.

## Example

```ts
function first<Item>(items: readonly Item[]): Item | undefined {
  return items[0];
}

function pluck<Item, Key extends keyof Item>(
  items: readonly Item[],
  key: Key,
): readonly Item[Key][] {
  return items.map((item) => item[key]);
}

// Library code: conditional type behind a named exported alias.
export type ElementOf<Value> = Value extends readonly (infer Item)[] ? Item : never;
```
