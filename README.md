# TypeScript Style Guide Skill

This repository packages the project owner's TypeScript conventions as a skill for AI coding agents. The guide is complete and maintained through the decision register and focused policy pages.

## Repository Map

Packaged skill content:

- [SKILL.md](SKILL.md): task router.
- [guidelines.md](guidelines.md): on-demand policy index.
- [guidelines/](guidelines): focused policy pages.
- [workflows/](workflows): procedures for larger tasks.

Maintenance content:

- [DECISIONS.md](DECISIONS.md): authoritative policy decisions.
- [OUTLINE.md](OUTLINE.md): policy ownership and decision references.
- [DRAFTING.md](DRAFTING.md): maintenance and page-writing rules.
- [TEMPLATE.md](TEMPLATE.md): guideline page shape.
- [AGENTS.md](AGENTS.md): repository instructions.
- [.ai/research/](.ai/research): background research, not policy.

## Maintenance

1. Amend [DECISIONS.md](DECISIONS.md) before changing policy.
2. Update the owning guideline or workflow and its entry in [OUTLINE.md](OUTLINE.md).
3. Update derived routing or checks when needed.
4. Run `bash checks/check.sh`.

## mdBook

The guide can be built as an mdBook for browsing:

```sh
mdbook build
mdbook serve --open
```

Book source lives under [src/](src). Symlinks expose the canonical packaged files, and a preprocessor strips `SKILL.md` frontmatter from the rendered book.

GitHub Pages deployment is handled by [.github/workflows/deploy.yml](.github/workflows/deploy.yml). Set the Pages source to "GitHub Actions" once; pushes to `main` deploy the generated book.
