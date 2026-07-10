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
- Make the rule direct and mechanical enough for an agent to apply.
- Add rationale, bullets, examples, or exceptions only when they add information.
- Keep rationale practical and examples small.
- Make incidental code follow the entire guide, especially its error and absence models.
- Put unresolved policy in `Decision Points` and [DECISIONS.md](DECISIONS.md), not in ordinary prose.

## Progressive Disclosure

- Keep [SKILL.md](SKILL.md) as a small task router.
- Keep [guidelines.md](guidelines.md) as the on-demand policy index.
- Put policy in `guidelines/` and multi-step procedures in `workflows/`.
- Link every packaged page directly from a router; avoid deep reference chains.
- Do not require unrelated pages for a task or duplicate their policy in a workflow.

## Scope

- Give agents conventions, not a TypeScript tutorial or an ecosystem survey.
- Add a page only when likely agent output needs a distinct owner.
- Keep planning notes and research outside the packaged skill.
- Do not edit `.ai/research/` as part of policy maintenance.
