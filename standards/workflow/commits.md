# Commit Standard

## Purpose

Commits are the structured interface to change history.

Write commits so reviewers, maintainers, future debuggers, release automation, changelog generators, and agents can understand what changed and why it matters. This standard defines one commit format for humans and automated agents.

Commits should make intent, affected scope, and release pressure visible without requiring a reader to inspect the full diff.

Use this standard when writing or reviewing commit messages. It covers commit title format, allowed tags, scope rules, gitmoji mapping, commit body rules, breaking change rules, release commit constraints, and the commit review checklist. It does not own the full release algorithm, full versioning rules, pull request structure, or tooling setup.

## Core Format

Every commit title must use this format:

```txt
<tag>(<scope[,scope...]>): <gitmoji> <title>
```

Git-history changelog tooling should parse this format. Configure changelog tools around the standard instead of changing commit titles to fit a default parser.

Examples:

```txt
docs(guides): 📝 add README guide
fix(parser): 🐛 handle empty input
feat(core,cli): ✨ add import preview command
```

The commit body is optional. When present, it must use bullet points only.

```txt
feat(parser): ✨ add incremental parsing

- ✨ support chunked input
- 🧪 cover partial token boundaries
```

Do not use paragraphs in commit bodies. If the change needs more explanation, use concise bullets.

## Components

### `tag`

The tag describes the kind of change and its semantic version pressure.

Use the tag that matches the user-visible or maintainer-visible effect of the change. Do not hide breaking changes under lower-impact tags.

### `scope`

The scope answers: what does this change apply to?

Use scopes for packages, modules, subsystems, directories, features, or `*` for repo-wide changes.

### `gitmoji`

The gitmoji is a scan marker for intent.

Each commit title must include exactly one primary gitmoji. The gitmoji must match the selected tag.

Body bullets may use gitmoji when the marker helps scan mixed changes, but they do not replace clear text.

### `title`

The title is a short imperative summary.

Use present tense. Do not end with punctuation unless the punctuation is part of a literal name.

```txt
Avoid:
fix(parser): 🐛 fixed parser issue.

Do:
fix(parser): 🐛 handle empty input
```

## Tags

Use these tags:

| Tag           | Purpose                                     | Scope    | Version pressure |
| ------------- | ------------------------------------------- | -------- | ---------------- |
| `BREAKING`    | Incompatible API or behavior change         | Required | Major            |
| `feat`        | New feature                                 | Required | Minor            |
| `fix`         | Bug fix                                     | Required | Patch            |
| `perf`        | Performance improvement                     | Required | Patch            |
| `deprecation` | Deprecate an existing API                   | Required | Patch            |
| `docs`        | Documentation only                          | Optional | Patch            |
| `refactor`    | Non-functional code change                  | Optional | Patch            |
| `test`        | Tests only                                  | Optional | Patch            |
| `style`       | Formatting or lint only                     | Optional | Patch            |
| `chore`       | Tooling, configuration, automation, or meta | Optional | Patch            |

This standard defines commit-level version pressure. [The versioning standard](versioning.md) owns full version meaning, and [the release standard](releases.md) owns the full release algorithm.

## Gitmoji Mapping

Use one primary gitmoji in the title. The selected gitmoji must match the tag.

| Gitmoji | Code                          | Intent                                      | Tag        |
| ------- | ----------------------------- | ------------------------------------------- | ---------- |
| 💥      | `:boom:`                      | Breaking change                             | `BREAKING` |
| ✨      | `:sparkles:`                  | New feature                                 | `feat`     |
| 🐛      | `:bug:`                       | Bug fix                                     | `fix`      |
| 🔥      | `:fire:`                      | Critical hotfix                             | `fix`      |
| 🔒      | `:lock:`                      | Security fix                                | `fix`      |
| ⚡      | `:zap:`                       | Performance improvement                     | `perf`     |
| 📝      | `:memo:`                      | Documentation                               | `docs`     |
| ♻️      | `:recycle:`                   | Refactor                                    | `refactor` |
| 🚚      | `:truck:`                     | Move or rename                              | `refactor` |
| 🗑️      | `:wastebasket:`               | Remove non-breaking code or files           | `refactor` |
| 🧪      | `:test_tube:`                 | Tests                                       | `test`     |
| 🎨      | `:art:`                       | Formatting                                  | `style`    |
| 🚨      | `:rotating_light:`            | Lint or warnings                            | `style`    |
| 🔧      | `:wrench:`                    | Configuration                               | `chore`    |
| 🔨      | `:hammer:`                    | Scripts or tooling                          | `chore`    |
| 👷      | `:construction_worker:`       | CI or automation                            | `chore`    |
| 📦      | `:package:`                   | Packaging or build output                   | `chore`    |
| ⬆️      | `:arrow_up:`                  | Upgrade dependency                          | `chore`    |
| ⬇️      | `:arrow_down:`                | Downgrade dependency                        | `chore`    |
| ➕      | `:heavy_plus_sign:`           | Add dependency                              | `chore`    |
| ➖      | `:heavy_minus_sign:`          | Remove dependency                           | `chore`    |
| 📌      | `:pushpin:`                   | Pin dependency                              | `chore`    |
| 🔀      | `:twisted_rightwards_arrows:` | Merge                                       | `chore`    |
| ⏪      | `:rewind:`                    | Revert                                      | `chore`    |
| 🎉      | `:tada:`                      | Project initialization or automated release | `chore`    |
| 🦇      | `:bat:`                       | Empty or novelty pre-history commit         | `chore`    |

Tooling may reject commits when the tag and gitmoji do not match. If a project automates commit validation, choose tooling according to [the tooling standard](../tooling.md).

```txt
Avoid:
docs(guides): ✨ add README guide

Do:
docs(guides): 📝 add README guide
```

## Scope Rules

Use the smallest useful scope.

Single-package repositories may use logical modules, subsystems, directories, or feature areas.

```txt
feat(parser): ✨ add streaming mode
feat(core/parser): ✨ add incremental tokenizer
docs(guides): 📝 add README guide
```

Multi-package repositories should use workspace package names when a change affects packages independently.

```txt
fix(core,cli): 🐛 handle empty input
docs(*): 📝 update terminology
```

Use `*` for repo-wide changes.

```txt
chore(*): 🔧 update shared tooling
```

## `unstable` Scope Modifier

Use `unstable` to mark changes to non-stable surface area.

When present, `unstable` must appear first in the scope path or scope list.

```txt
feat(unstable/parser): ✨ add draft streaming API
fix(unstable/core,cli): 🐛 handle experimental flag parsing
```

Do not put `unstable` at the end of a scope.

```txt
Avoid:
feat(parser/unstable): ✨ add draft streaming API

Do:
feat(unstable/parser): ✨ add draft streaming API
```

The `unstable` modifier signals patch-only release impact. It does not make an invalid commit valid, and it does not hide breaking changes to stable APIs.

## Body Rules

Use a body when the commit changes multiple concerns, carries risk, or needs more detail than the title can hold.

Body rules:

- Use bullet points only.
- Describe one atomic change per bullet.
- Keep each bullet concrete.
- Do not include version numbers.
- Do not include release intent in normal commits.
- Do not hide breaking changes in the body.

```txt
Avoid:
docs(guides): 📝 add writing guides

This adds some docs and moves things around. It should be ready for release.

Do:
docs(guides): 📝 add writing guides

- 📝 add README guide
- 📝 add TypeScript JSDoc guide
```

## Breaking Changes

Breaking changes must use `BREAKING` in the title.

Do not rely on the body to reveal a breaking change. A reader scanning history or a release tool calculating version impact must see the breaking change from the title.

```txt
Avoid:
feat(api): ✨ rename createClient

- 💥 rename `createClient` to `createApiClient`

Do:
BREAKING(api): 💥 rename createClient
```

## Release Commits

Release commits are reserved for automation.

```txt
chore(*): 🎉 release vX.Y.Z
```

Release commits:

- Must be generated by automation.
- Must use `chore(*)`.
- Must not affect version bump calculation.
- Must be the only normal place where commit text names a release version.

[The release standard](releases.md) owns the full release process.

## BATMAN Carve-Out

The bat emoji is allowed only for empty or novelty pre-history commits with no meaningful project content. If the first commit adds real code, documentation, configuration, or project structure, use the normal tag and gitmoji.

```txt
chore(meta): 🦇 BATMAN
```

Do not use `BATMAN` for meaningful first commits.

```txt
Avoid:
chore(meta): 🦇 BATMAN

- 📝 add initial writing principles guide

Do:
docs(guides): 📝 add writing principles
```

## Examples

### Documentation

```txt
docs(guides): 📝 add README guide

- 📝 define README structure and required sections
- 📝 document visual proof, status, badges, and scan markers
```

### Feature

```txt
feat(imports): ✨ add dry-run import preview
```

### Bug Fix

```txt
fix(parser): 🐛 handle empty CSV rows
```

### Breaking Change

```txt
BREAKING(api): 💥 rename createClient
```

### Unstable Surface

```txt
feat(unstable/parser): ✨ add draft streaming API
```

### Invalid Body

```txt
Avoid:
fix(parser): 🐛 handle empty input

This fixes a parsing issue and updates tests.

Do:
fix(parser): 🐛 handle empty input

- 🐛 return an empty result for blank files
- 🧪 cover blank-file parsing
```

## Review Checklist

Use this checklist before committing:

- Does the title match `<tag>(<scope[,scope...]>): <gitmoji> <title>`?
- Does the tag match the change type?
- Does the gitmoji match the tag?
- Does the scope answer what the change applies to?
- Is the title short, specific, imperative, and present tense?
- Is the body bullet-only when present?
- Does each body bullet describe one atomic change?
- Is any breaking change visible through `BREAKING` in the title?
- Is release intent absent from normal commits?
- Is `unstable` first when used?
