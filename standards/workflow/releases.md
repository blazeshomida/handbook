# Release Standard

## Purpose

Releases turn reviewed changes into versioned artifacts users can trust.

Use this standard to run and review releases after version impact has already
been classified. It covers the release checklist, changelog expectations,
release notes expectations, release tags, publishing expectations, release
artifacts, prerelease flow, and post-release verification. It does not own
commit title format, version meaning, or pull request structure.

Use [the commit standard](commits.md) for commit title format and release commit
constraints. Use [the versioning standard](versioning.md) for version meaning
and change classification. Use
[the release notes template](../../templates/releases/release-notes.md) for
copyable release note structure.

## Release Checklist

Before publishing a release:

- Confirm the working tree is clean.
- Confirm all intended changes are merged.
- Confirm release-relevant commits are classified correctly.
- Confirm the version impact from [the versioning standard](versioning.md).
- Run the project's required checks.
- Generate or review changelog output.
- Draft release notes when users need a user-facing summary.
- Confirm release artifacts are complete.
- Publish only after tags, artifacts, and notes can be kept consistent.

Do not publish from a dirty tree unless the project has an explicit release mode
that does not use the working tree as the source of truth.

## Release Modes

Each repository should choose one release mode.

Use single-package mode when the repository releases one version for the whole
repo.

Single-package releases have:

- One version for the repo.
- One changelog.
- One release tag per release.

Use multi-package mode when packages in the repository release independently.

Multi-package releases have:

- One version per package.
- One changelog per package.
- One release tag per released package.

Document the selected mode in the project release configuration or release docs.
Do not mix single-package and multi-package release behavior without an explicit
migration plan.

## Release Tags

Tags are the source of truth for released versions.

Valid single-package tag format:

```txt
vX.Y.Z
```

Valid multi-package tag format:

```txt
<pkg>/vX.Y.Z
```

Examples:

```txt
v1.4.0
core/v2.1.3
cli/v0.9.1
```

Invalid tags:

```txt
release-v1.0.0
v1
core-1.2.3
```

Do not publish a release without the expected tag. In normal release mode, tags
should point to the release commit. In tag-only mode, tags should point to the
source commit being released.

## Release Artifacts

A release is complete only when all required artifacts agree.

Typical release artifacts include:

- Version tags.
- Published packages or deployable builds.
- Manifest version updates when the project uses manifest versions.
- Changelog output.
- Release notes when users need a readable summary.
- Generated docs or API references when the project publishes them.

Do not leave a repository in a state where tags, manifests, changelogs, release
notes, and published artifacts disagree about the released version.

## Changelogs

Generate changelogs from Git history between release tags.

Changelog output should:

- Exclude release commits.
- Group entries by stable commit-tag categories.
- Render commit titles verbatim, including gitmoji.
- Keep group ordering stable across releases.
- Point readers to the release notes when a human summary is needed.

Example grouping:

```md
## Features

- feat(imports): ✨ add dry-run import preview

## Fixes

- fix(parser): 🐛 handle empty CSV rows

## Documentation

- docs(guides): 📝 add README guide
```

Use [the tooling standard](../tooling.md) for changelog tooling choices. Do not
hand-edit generated changelog content unless the generated output is wrong and
the source commit history or generator configuration is also fixed.

## Release Notes

Release notes are user-facing release communication.

Write release notes when users need more than a generated changelog to
understand impact, migration steps, risk, or upgrade value.

Release notes should include:

- The released version.
- The most important changes.
- Breaking changes and migration notes.
- Known risks or compatibility notes.
- Links to relevant docs, issues, or pull requests when useful.

Do not use release notes to hide incorrect commit classification or version
impact. Fix the commit or versioning source before release.

## Publishing Expectations

Publishing should be repeatable and explainable.

Before publishing:

- Run the project's release checks.
- Confirm package or artifact metadata is complete.
- Confirm the publish command will use the intended files.
- Confirm release tags can be created.
- Confirm generated changelog and release notes are ready.

After publishing:

- Confirm the package, artifact, or deployment is visible in the expected
  registry, host, or environment.
- Confirm the published version matches the tag.
- Confirm changelog and release notes point to the published version.
- Confirm no unpublished local release changes remain.

If any required artifact cannot be created, stop and repair the release before
publishing.

## Prerelease Flow

Use prereleases for early access, release candidates, compatibility testing, and
coordinated ecosystem validation.

Prerelease flow should:

- Use the version impact from [the versioning standard](versioning.md).
- Use a clear prerelease identifier such as `alpha`, `beta`, or `rc`.
- Keep changelog and release notes clear that the version is not stable.
- Promote to a stable release only after prerelease validation is complete.

Do not treat prereleases as a way to avoid documenting breaking changes or
migration work.

## Release Commits

Release commits are generated only by automation.

Single-package release commit:

```txt
chore(*): 🎉 release vX.Y.Z
```

Multi-package release commit:

```txt
chore(*): 🎉 release
```

Release commits:

- Must use `chore(*)`.
- Must not affect version bump calculation.
- Must be the only commits containing manifest version updates.
- Must be the only commits containing generated changelog updates.

Do not include release intent in normal commits or pull request titles.

## Tag-Only Releases

Tag-only releases are an explicit project choice.

In tag-only mode:

- No manifest version updates are committed.
- Tags are the version source.
- Changelogs may be generated at release time instead of committed.
- Release commits may be omitted.
- Tags point to the source commit being released.

Document tag-only mode before using it. Tag-only releases have weaker
compatibility with ecosystem tooling that expects manifest versions.

## Post-Release Verification

After release, verify:

- The expected tag exists and points to the intended commit.
- Published artifacts are visible.
- Published versions match the release tag.
- Changelog output includes the released changes.
- Release notes are attached or published when expected.
- Installation, import, or deployment smoke checks pass when applicable.
- The repository has no leftover release changes.

Treat failed post-release verification as release work. Fix the release state or
publish a corrective release instead of leaving the inconsistency undocumented.

## Review Checklist

Use this checklist before completing a release:

- Is the release mode clear?
- Is the version impact classified by the versioning standard?
- Are release-relevant commits classified correctly?
- Are required checks passing?
- Are tags in the expected format?
- Do tags, manifests, changelogs, notes, and artifacts agree?
- Is the changelog generated from Git history?
- Are breaking changes and migration notes visible in release notes?
- Is the publish target correct?
- Has post-release verification passed?
