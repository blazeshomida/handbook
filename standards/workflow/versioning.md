# Versioning Standard

## Purpose

Versions communicate compatibility and change impact.

Use this standard to classify changes and understand semantic version meaning.
It covers patch, minor, and major classification; prerelease versions; breaking
changes; unstable surface area; versioning examples; and the relationship
between versions and commit tags. It does not own the full release checklist,
release notes template, or publishing process.

Use [the commit standard](commits.md) for commit tags and title format. Use
[the release standard](releases.md) for the release process. When it exists,
use `templates/releases/changeset.md` for copyable change declarations.

## Semantic Versioning

Use Semantic Versioning:

```txt
MAJOR.MINOR.PATCH
```

Interpret version parts as:

| Part | Meaning |
| --- | --- |
| `MAJOR` | A stable public API or behavior changed incompatibly. |
| `MINOR` | A stable public feature or capability was added compatibly. |
| `PATCH` | A compatible fix, documentation change, refactor, test change, tooling change, or unstable-surface change landed. |

Version numbers should describe user-visible and maintainer-visible impact, not
the amount of code changed.

## Commit Tags And Version Pressure

Commit tags create version pressure.

Use this mapping:

| Commit tag | Default version pressure |
| --- | --- |
| `BREAKING` | Major |
| `feat` | Minor |
| `fix` | Patch |
| `perf` | Patch |
| `deprecation` | Patch |
| `docs` | Patch |
| `refactor` | Patch |
| `test` | Patch |
| `style` | Patch |
| `chore` | Patch |

If version pressure is wrong, fix the commit classification before release.
Do not use release notes, pull request text, or manual release intent to hide an
incorrect commit tag.

## Patch Changes

Use a patch version for compatible changes that should not require users to
change how they call or depend on stable APIs.

Patch changes include:

- Bug fixes.
- Compatible performance improvements.
- Documentation-only changes.
- Test-only changes.
- Formatting or lint-only changes.
- Tooling, configuration, automation, or metadata changes.
- Compatible refactors.
- Deprecations that do not remove behavior.
- Changes limited to unstable surface area.

Examples:

```txt
fix(parser): 🐛 handle empty CSV rows
docs(guides): 📝 document import workflow
deprecation(api): 📝 deprecate legacy client factory
```

## Minor Changes

Use a minor version for compatible additions to stable public behavior.

Minor changes include:

- New stable public APIs.
- New stable commands, options, routes, or features.
- Compatible additions to supported data formats.
- New stable capabilities that users can adopt without changing existing usage.

Examples:

```txt
feat(imports): ✨ add dry-run import preview
feat(cli): ✨ add json output mode
```

Do not use `feat` for internal-only implementation changes that do not expose a
new stable capability.

## Major Changes

Use a major version for incompatible changes to stable public behavior.

Major changes include:

- Removing or renaming a stable public API.
- Changing required inputs or output shapes incompatibly.
- Changing stable command, route, permission, or data format behavior
  incompatibly.
- Removing a supported runtime, platform, or integration.
- Changing defaults in a way that can break existing users.

Examples:

```txt
BREAKING(api): 💥 rename createClient
BREAKING(cli): 💥 remove legacy import command
```

Breaking changes must be visible in the commit title with `BREAKING`. Do not
hide breaking changes in commit bodies, pull request descriptions, or release
notes.

## Unstable Surface Area

Use the `unstable` scope modifier for non-stable surface area.

When `unstable` appears first in the scope path or scope list, that commit
creates patch pressure even if the tag is `feat` or `BREAKING`.

```txt
feat(unstable/parser): ✨ add draft streaming API
BREAKING(unstable/parser): 💥 rename draft streaming API
```

The `unstable` modifier does not make an invalid commit valid, and it does not
hide breaking changes to stable APIs.

If one release target contains both stable and unstable work, stable version
pressure wins for that target.

## Prerelease Versions

Use prerelease versions for versions that should be available before a stable
release but should not carry stable compatibility expectations.

Prerelease identifiers attach to a normal semantic version:

```txt
1.4.0-alpha.1
2.0.0-rc.1
```

Use prereleases for:

- Early access to a planned stable feature.
- Release candidate validation.
- Compatibility testing before a major release.
- Coordinated ecosystem testing.

Prereleases do not remove the need to classify the underlying change. A
prerelease for a breaking stable change still points toward a major stable
version.

## Change Classification Examples

| Change | Commit title | Version pressure |
| --- | --- | --- |
| Fix empty CSV parsing | `fix(parser): 🐛 handle empty CSV rows` | Patch |
| Add a stable import preview | `feat(imports): ✨ add dry-run import preview` | Minor |
| Rename a stable client API | `BREAKING(api): 💥 rename createClient` | Major |
| Add a draft parser API | `feat(unstable/parser): ✨ add draft streaming API` | Patch |
| Rename a draft parser API | `BREAKING(unstable/parser): 💥 rename draft streaming API` | Patch |
| Update documentation | `docs(guides): 📝 document import workflow` | Patch |
| Deprecate a stable API | `deprecation(api): 📝 deprecate legacy client factory` | Patch |

## Review Checklist

Use this checklist when classifying version impact:

- Is the affected surface stable or unstable?
- Does any stable public behavior change incompatibly?
- Does a stable public capability get added compatibly?
- Is the commit tag aligned with the highest version pressure in the change?
- Is every breaking stable change marked with `BREAKING`?
- Is `unstable` first in the scope when used?
- Are docs, tests, style, refactors, chores, and deprecations treated as patch
  pressure unless they hide stable behavior changes?
- Are prerelease versions still tied to the underlying major, minor, or patch
  classification?
