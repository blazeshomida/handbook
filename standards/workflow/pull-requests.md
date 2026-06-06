# Pull Request Standard

## Purpose

Pull requests are the review interface for a change.

Use a pull request to explain what changed, why it matters, how it was verified,
and what reviewers should inspect before the change becomes project history.

Use this standard when writing, reviewing, or deciding whether to open a pull
request. It covers pull request size, titles, bodies, visual proof, testing
notes, linked issues, reviewability, breaking change notes, and when not to open
or mark a pull request ready. It does not own commit format, the versioning
algorithm, or the release process.

Use [the commit standard](commits.md) for commit and pull request title format.
Use [the versioning standard](versioning.md) for version-impact details. When
it exists, use `templates/github/pull_request_template.md` for the copyable
GitHub form.

## Core Requirements

Every pull request must include:

- A structured title.
- A short summary of the change.
- The important changes reviewers should understand.
- Verification evidence.
- Screenshots, recordings, demos, or output when behavior is visual or hard to
  infer from text.
- Breaking changes, migration notes, or risks when they exist.

Do not rely on comments, commit bodies, or private context to explain the pull
request. Keep the description current as the scope changes.

## Title

Pull request titles must use the same title format as commits:

```txt
<tag>(<scope[,scope...]>): <gitmoji> <title>
```

The title should describe the primary reviewable change. For a single-commit
pull request, the pull request title should usually match the commit title.

For a multi-commit pull request, use the title that best describes the combined
effect. Do not list every commit in the title.

```txt
Avoid:
feat(imports): ✨ add import preview and fix parser and update docs

Do:
feat(imports): ✨ add dry-run import preview
```

Do not include release intent in a pull request title.

```txt
Avoid:
feat(imports): ✨ release v1.3.0 import preview

Do:
feat(imports): ✨ add dry-run import preview
```

## Size And Scope

Keep pull requests focused.

A focused pull request changes one reviewable concern. It may include supporting
tests, documentation, configuration, or cleanup when those changes are necessary
to complete the concern.

Split a pull request when it mixes independent concerns, creates avoidable
review risk, or requires different reviewers to approve unrelated changes.

```txt
Avoid:
feat(imports): ✨ add import preview

- Add import preview.
- Rename the auth package.
- Rewrite the dashboard layout.

Do:
feat(imports): ✨ add dry-run import preview
```

Use stacked or follow-up pull requests when a change cannot stay small without
losing useful review order.

## Body

Use this order unless another order makes the review easier:

1. Summary.
2. Changes.
3. Verification.
4. Screenshots, recordings, demos, or output when useful.
5. Breaking changes, migration notes, or risks.
6. Linked issues or decisions.
7. Follow-ups.

Omit sections that do not apply. Do not keep empty headings.

The summary should explain the result of the change, not the process of making
it.

```md
Avoid:
This pull request updates parser files and fixes issues found while testing
imports.

Do:
This pull request adds a dry-run import preview so users can inspect parsed rows
before writing records.
```

List important reviewable changes as concrete bullets. Do not turn the changes
section into a file list; reviewers can see files in the diff.

```md
- ✨ Adds a dry-run import mode that returns parsed rows without writing records.
- 🐛 Rejects CSV files that are missing required headers.
- 📝 Documents the preview workflow.
```

## Verification

Show how the change was checked.

Include exact commands and relevant manual checks. A reviewer should be able to
repeat the important verification without guessing.

````md
```sh
deno test
deno lint
```

- Confirmed dry-run imports do not write records.
- Confirmed invalid CSV headers show a validation error.
````

If verification was not run, say that directly and explain why.

```md
Not run. Documentation-only change.
```

Do not use vague verification notes such as `Tested locally`.

## Visual Proof

Add visual proof when it helps reviewers evaluate the change faster than text.

Use visual proof for:

- UI changes.
- CLI output changes.
- Documentation rendering changes.
- Generated output.
- Workflow changes with visible state transitions.

Use the most direct proof:

- Screenshot for UI state.
- GIF or short recording for motion or interaction.
- Terminal output for CLIs.
- Before-and-after examples for transformations.

Visual proof must be current, readable, and specific.

```md
Avoid:
Screenshot attached.

Do:
![Import preview showing 240 parsed rows and 3 validation warnings](./docs/import-preview.png)
```

Do not use stale screenshots, decorative images, or cropped captures that hide
the changed behavior.

## Linked Issues And Decisions

Link related issues, discussions, specifications, incidents, or decisions when
they explain required review context.

Do not link issues as a substitute for summarizing the current change. The pull
request body should still explain what changed and how it was verified.

Use closing keywords only when the pull request fully resolves the issue.

## Breaking Changes And Risk

Make breaking changes visible.

If a pull request changes a stable public API, data format, command, route,
permission model, or user-visible behavior incompatibly, the title must use
`BREAKING`.

```txt
BREAKING(api): 💥 rename createClient
```

Add migration notes when a user or maintainer must take action.

```md
> [!WARNING]
> `createClient` has been renamed to `createApiClient`. Update imports before
> upgrading.
```

Call out risk when it changes review focus or rollout behavior.

```md
> [!CAUTION]
> This changes import validation before records are written. Review rejected row
> handling carefully.
```

Do not hide breaking changes or risk in comments after review has started.
Update the pull request description.

## When Not To Open Or Mark Ready

Do not open a normal ready-for-review pull request when:

- The change has no useful title or summary yet.
- Required implementation work is still missing.
- Verification is missing and there is no explanation.
- The pull request mixes unrelated concerns that can be split.
- Breaking changes or migration notes are known but not described.
- Visual proof is required for review but not included.

Use a draft pull request for early feedback, CI checks, or visible work in
progress. A draft should still have a useful title and summary, and it should
make the review status obvious.

```md
> [!NOTE]
> Draft: API shape is ready for feedback. Tests and documentation are still in
> progress.
```

Convert a draft to ready for review only when the description, verification, and
known blockers are current.

## Follow-Ups

Use follow-ups only for intentionally deferred work.

A follow-up should be concrete enough to become an issue, pull request, or task.
Do not use follow-ups to excuse incomplete required work.

```md
- Add import preview metrics in a follow-up pull request.
- Move legacy CSV normalization behind a feature flag.
```

If a follow-up is required before merge, use a task list and keep it visible.

```md
- [ ] Add migration note for renamed CLI flag.
- [ ] Confirm dashboard screenshot after copy changes.
```

## GitHub Markdown

Use GitHub Markdown features deliberately.

| Feature | Use it for | Avoid |
| --- | --- | --- |
| Alerts | Breaking changes, warnings, rollout risk | Routine information |
| Task lists | Merge blockers and reviewable remaining work | General notes |
| Tables | Comparisons, matrices, before/after behavior | Long prose |
| Images and GIFs | UI, CLI, docs, and generated output proof | Decoration |
| Details blocks | Long logs or optional evidence | Hiding required context |
| Emojis | Scan markers in lists and tables | Ambiguous decoration |

Do not make ordinary context look urgent.

## Merge Readiness

A pull request is ready to merge when:

- The title follows commit format.
- The description explains what changed and why.
- The scope is focused.
- Verification is listed.
- Screenshots, recordings, demos, or output are included when needed.
- Breaking changes and migration notes are explicit.
- Risk is visible.
- Required follow-ups are complete or intentionally deferred.
- Review comments are addressed.
- CI is passing or failures are explained.

Do not merge a pull request whose description disagrees with the final diff.

## Review Checklist

Use this checklist before requesting review:

- Does the title match `<tag>(<scope[,scope...]>): <gitmoji> <title>`?
- Does the title describe the primary reviewable change?
- Does the summary explain the result of the change?
- Are the important changes listed?
- Is the scope focused?
- Is verification exact and repeatable?
- Is missing verification explained?
- Is visual proof included when it helps review?
- Are breaking changes visible in the title and description?
- Are migration notes included when users must take action?
- Is risk called out when it changes review or rollout behavior?
- Are linked issues or decisions relevant to the review?
- Are follow-ups concrete and intentionally deferred?
- Are GitHub alerts, tables, task lists, and emojis used only when useful?
- Is the description current with the final diff?
