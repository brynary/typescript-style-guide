# TypeScript Style Guide Skill Outline

This outline is for a TypeScript style guide packaged as an AI-agent skill. The skill should not teach TypeScript syntax or re-explain the language handbook. It should give agents clear defaults, exceptions, and small examples for the decisions they face while writing TypeScript code.

Target size: to be decided. The sibling guides use roughly 24-40 guideline pages plus 4 workflow pages; each guideline should fit on one focused markdown page.

## Drafting Instructions

Use [DRAFTING.md](DRAFTING.md) for drafting order, scope, and page-writing guidance.

## Page Template

Use [TEMPLATE.md](TEMPLATE.md) for every guideline page.

## Packaged Skill Shape

The final skill should follow the progressive-disclosure pattern in [SKILL.md](SKILL.md) and [guidelines.md](guidelines.md): a small root router, focused guideline pages, and task workflows under `workflows/`. Planning files and research reports should not be part of the packaged skill by default.

## Decision Register

Use [DECISIONS.md](DECISIONS.md) to resolve style decisions before drafting final policy pages.

## Guideline Map

Not yet written. Populate with numbered topics grouped into sections, each topic listing its scope bullets and decision-point references (see the rust-style-guide and postgres-style-guide outlines for the shape).

## Workflows

Not yet written. List the planned workflow pages (procedures for repeated larger tasks) with a one-line scope each.
