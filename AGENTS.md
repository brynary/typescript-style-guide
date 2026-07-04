# AGENTS.md

## Project Purpose

This repository is a TypeScript style guide packaged as a skill for AI coding agents. The guide should help agents write idiomatic TypeScript using the project owner's conventions.

The repository is currently an infrastructure skeleton: the decision register, outline, and policy pages have not been written yet.

Keep the work simple, explicit, and useful for agents. Do not turn the guide into a TypeScript textbook.

## Key Files

- [OUTLINE.md](OUTLINE.md): guideline map for the full style guide.
- [DECISIONS.md](DECISIONS.md): style decision register.
- [DRAFTING.md](DRAFTING.md): drafting order, scope rules, and page-writing guidance.
- [TEMPLATE.md](TEMPLATE.md): required guideline page format.
- [SKILL.md](SKILL.md): skill entrypoint and root router.
- [guidelines.md](guidelines.md): guideline index for progressive disclosure.
- [guidelines/](guidelines): focused TypeScript style policy pages.
- [workflows/](workflows): procedural workflows for larger tasks.
- [checks/](checks): validation harness for the packaged skill.
- [.ai/research/](.ai/research): source research reports used to create the outline.

## Working Rules

- Read [DECISIONS.md](DECISIONS.md) before drafting or changing policy pages; add or amend a register entry before changing policy.
- Use [TEMPLATE.md](TEMPLATE.md) for every guideline page.
- Follow [DRAFTING.md](DRAFTING.md) for drafting order, scope, and the one-owner-per-rule principle.
- Keep guideline pages short, concrete, and mechanical enough for an agent to follow.
- Put unresolved choices in `Decision Points` instead of hiding them in prose.
- Keep [SKILL.md](SKILL.md) small. Put detailed policy in `guidelines/` and procedures in `workflows/`.
- Link guideline and workflow files directly from [SKILL.md](SKILL.md) or [guidelines.md](guidelines.md); avoid deep reference chains.
- Run `bash checks/check.sh` before committing skill changes.
- Do not edit files in [.ai/research/](.ai/research) unless explicitly asked.

## Style Guide Bias

Not yet decided. Record the overall posture here (a short bullet list of the guide's opinionated defaults) once the decision register in [DECISIONS.md](DECISIONS.md) is resolved.

## Editing Expectations

- Preserve ASCII-only markdown unless a file already uses non-ASCII intentionally.
- Keep changes narrowly scoped to the requested document.
- Avoid adding new guideline or workflow files unless the user asks for new pages.
- When adding or changing decisions, update [DECISIONS.md](DECISIONS.md) and keep topic references in [OUTLINE.md](OUTLINE.md) consistent.
- Do not include planning docs or research reports in the packaged skill unless explicitly requested.
