# Templates

Templates are copyable files that help projects apply the handbook standards.

Copy a template when a project needs the file shape. Replace placeholders before
using the copied file. Template links point back to the canonical handbook
standards on GitHub.

## Available Templates

- [Agent instructions](agents/AGENTS.md): project-level instructions for agents
  and contributors working in a repo.
- [Pull request template](github/pull_request_template.md): GitHub pull request
  body structure for reviewable changes.
- [Changeset](releases/changeset.md): Changesets entry structure for package
  version intent and user-facing impact.
- [Release notes](releases/release-notes.md): user-facing release notes
  structure.

## Related Standards

- [Agent instructions](agents/AGENTS.md) points to the
  [commit](../standards/workflow/commits.md),
  [TypeScript](../standards/code/typescript.md),
  [code documentation](../standards/code/documentation.md), and
  [tooling](../standards/tooling.md) standards.
- [Pull request template](github/pull_request_template.md) applies the
  [pull request standard](../standards/workflow/pull-requests.md).
- [Changeset](releases/changeset.md) applies the
  [versioning standard](../standards/workflow/versioning.md).
- [Release notes](releases/release-notes.md) applies the
  [release standard](../standards/workflow/releases.md).

## When To Copy Templates

Copy a template when the target project needs that exact file type. After
copying:

- Replace placeholders with project-specific content.
- Remove sections that do not apply.
- Keep links to relevant standards useful after copy.
- Adjust commands, package names, and release details to the target project.

## Boundary

These are file-level templates, not project templates.

Do not use this directory for:

- Full starter apps.
- Framework scaffolds.
- Package boilerplates.
- Generated project trees.
- Tool configuration presets.

Project templates belong in a separate repo or package.
