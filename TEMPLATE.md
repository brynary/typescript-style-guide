# Guideline Page Template

Use this format for TypeScript style guide guideline pages.

```markdown
# Guideline Name

## Rule

One sentence the agent can follow by default.

## Why

Short rationale.

## Do

- Preferred patterns.
- Naming or API conventions.

## Avoid

- Common anti-patterns.
- Cases where agents usually overreach.

## Example

Small preferred TypeScript example.

## Exceptions

Narrow cases where the default can be broken.
```

Optional sections:

- `Activation`
- `Decision Points`

Use `Activation` for conditional or advanced guideline pages. It should say when to load the page and when not to.

Use `Decision Points` for unresolved choices that must be settled in [DECISIONS.md](DECISIONS.md).

Add further optional sections (the sibling guides use domain-specific ones such as `Version Notes` or `Library vs Application`) as the decision register settles which dimensions matter for TypeScript.
