# Standards

Standards define reusable engineering policy for code, documentation, workflow, and release work.

Use standards when deciding how to write or review something. Do not use them as project templates, setup scripts, or exhaustive tutorials.

## Tooling

- [Tooling](tooling.md): default language, runtime, formatting, linting, testing, build, dependency, and release-tool preferences.

## Code Standards

- [Code documentation](code/documentation.md): when to document code, what to document, and when comments should be omitted.
- [JavaScript](code/javascript.md): runtime-agnostic JavaScript patterns for naming, modules, functions, async code, errors, values, iteration, comments, and file organization.
- [TypeScript](code/typescript.md): TypeScript-specific rules for strictness, public API types, runtime boundaries, casts, generics, unions, imports, and exports.
- [JSDoc](code/jsdoc.md): JSDoc tags, formatting, examples, deprecations, internal API documentation, and links.

## Workflow Standards

- [Commits](workflow/commits.md): commit title format, tags, scopes, gitmoji, bodies, breaking changes, release commits, and review checklist.
- [Pull requests](workflow/pull-requests.md): pull request titles, summaries, changes, verification, visual proof, risks, breaking changes, and review readiness.
- [Versioning](workflow/versioning.md): semantic version meaning, version pressure, prereleases, breaking changes, unstable surface area, and change classification.
- [Releases](workflow/releases.md): release checklist, changelog expectations, release notes, tags, artifacts, publishing, prereleases, and post-release verification.

## Dependency Order

Read standards in the order that matches the work:

1. Use [Tooling](tooling.md) for tool and dependency choices.
2. Use [Code documentation](code/documentation.md) before syntax-specific API documentation.
3. Use [JavaScript](code/javascript.md) before [TypeScript](code/typescript.md).
4. Use [JSDoc](code/jsdoc.md) after the documentation, JavaScript, and TypeScript standards.
5. Use [Commits](workflow/commits.md) before pull requests, versioning, and releases.
6. Use [Pull requests](workflow/pull-requests.md) before merge review.
7. Use [Versioning](workflow/versioning.md) before release work.
8. Use [Releases](workflow/releases.md) when publishing versioned artifacts.

## How To Use Standards

- Start with the standard that owns the decision.
- Follow links when another file owns a related rule.
- Prefer project-specific overrides only when the project has a concrete reason.
- Keep standards focused on durable policy, not one-off project setup.

## What Not To Put Here

- Copyable file templates.
- Project scaffolds or starter apps.
- Tool configuration files.
- Repo-specific checklists that belong in a project.
- Detailed tutorials that do not change the standard.
