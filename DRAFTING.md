# Drafting Instructions

Use these instructions when turning the outline into guideline pages.

## Priorities

1. Resolve the decision register in [DECISIONS.md](DECISIONS.md) before drafting policy pages, flagship decisions first.
2. Keep [SKILL.md](SKILL.md) small and use it as the router.
3. Draft the core guideline pages first; define the core set in [OUTLINE.md](OUTLINE.md).
4. Add workflow pages only where repeated task procedures need more than policy.
5. Add advanced guideline pages only where the target codebases need them.
6. Keep every page mechanical enough for an agent to follow.

## Drafting Order

Not yet defined. Once [OUTLINE.md](OUTLINE.md) has a guideline map, list its sections here in drafting order, foundations first.

## Page Rules

- Use [TEMPLATE.md](TEMPLATE.md) for every guideline page.
- Give every rule exactly one owner page; sibling pages may carry at most a one-line reminder that links to the owner.
- The review workflow's checklist derives from guideline pages; a policy change that adds or removes a ban must update it in the same change.
- Make examples demonstrate only the owning page's rules; incidental code in an example follows other pages' rules but does not showcase them.
- Make the `Rule` section a direct default, not a discussion.
- Keep `Why` short and practical.
- Prefer concrete guidance over philosophy.
- Include exceptions only when an agent could reasonably encounter them.
- Add a small preferred TypeScript example when the topic affects code shape.
- Put unresolved choices in `Decision Points` instead of burying them in prose.

## Progressive Disclosure

- Treat [SKILL.md](SKILL.md) as the skill entrypoint and root router, not the guide itself.
- Keep detailed policy in `guidelines/` pages.
- Keep procedural task flows in `workflows/` pages.
- Keep [guidelines.md](guidelines.md) as the one-page guideline index.
- Link guideline and workflow files directly from [SKILL.md](SKILL.md) or [guidelines.md](guidelines.md); avoid deep reference chains.
- Do not load every guideline page for ordinary tasks.
- Use routing examples for common task types so agents know which pages to load.

## Scope Rules

- Do not re-teach TypeScript syntax or type-system basics unless a style choice depends on them.
- Do not include long surveys of ecosystem options on guideline pages.
- Do not add advanced topics unless they affect likely agent output.
- Add domain-specific scope rules here (audience, strictness splits, runtime assumptions) as the decision register resolves them.
- Do not package planning files or research reports into the final skill unless the user explicitly asks for them.
