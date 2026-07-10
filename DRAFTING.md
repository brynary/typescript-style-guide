# Maintenance Instructions

Use these instructions when changing the completed style guide.

## Policy Changes

1. Add or amend the relevant row in [DECISIONS.md](DECISIONS.md).
2. Update the one guideline page that owns the rule.
3. Keep the page's decision references in [OUTLINE.md](OUTLINE.md) current.
4. Update derived configuration, routing, workflows, or checks only when the rule affects them.
5. Run `bash checks/check.sh`.

## Page Rules

- Use [TEMPLATE.md](TEMPLATE.md); only `Rule` is universal, and overlays also require `Activation`.
- Give every rule one owner page. Sibling pages may use one linked reminder when needed.
- Keep guideline pages at or below 100 lines.
- Make the rule direct and mechanical enough for an agent to apply.
- Add rationale, bullets, examples, or exceptions only when they add information.
- Keep rationale practical and examples small.
- Make incidental code follow the entire guide, especially its error and absence models.
- Put unresolved policy in `Decision Points` and [DECISIONS.md](DECISIONS.md), not in ordinary prose.

## Routing Contract

- `SKILL.md` owns workflow discovery, direct policy fast paths, and the fallback to `guidelines.md`.
- `guidelines.md` indexes policy pages only; it never links workflows.
- Each workflow appears exactly once in `SKILL.md`.
- Each guideline appears exactly once in `guidelines.md`.
- Workflow tasks route to the workflow alone; the workflow selects policy pages in `Guideline Routing`.
- Routing descriptions state when to select overlays; their `Activation` sections confirm whether to apply or skip them after loading.
- Avoid deep reference chains, unrelated co-loads, and duplicated workflow policy.

## Workflow Format

- `Guideline Routing` identifies always-loaded and conditional policy pages.
- `Workflow` contains the procedure.
- `Avoid` is optional and includes only workflow-specific failure modes.

## Scope

- Give agents conventions, not a TypeScript tutorial or an ecosystem survey.
- Add a page only when likely agent output needs a distinct owner.
- Keep planning notes and research outside the packaged skill.
- Do not edit `.ai/research/` as part of policy maintenance.
