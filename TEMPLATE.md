# Guideline Page Template

Every guideline page needs a direct `Rule`. Add other sections only when they contribute information the rule does not already contain.

```markdown
# Guideline Name

## Rule

One sentence the agent can follow by default.
```

Optional sections, in this order when present:

- `Activation`: required for overlays; state when to load and skip the page.
- `Why`: one short, practical rationale.
- `Do`: mechanical implications not already stated by the rule.
- `Avoid`: likely mistakes that are not merely the inverse of `Do`.
- `Example`: the smallest example that clarifies code shape.
- `Exceptions`: genuine cases where the rule changes; omit when there are none.
- `Decision Points`: unresolved policy that must be settled in [DECISIONS.md](DECISIONS.md).

Do not add a section that repeats another section. Every example must follow the rest of the guide, not only the page it illustrates.
