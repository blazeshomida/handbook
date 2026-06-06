# Agent Instructions

## Project Overview

[Describe what this project builds, who it is for, and the main runtime or
framework.]

## Commands

Use the project package manager and runtime.

```sh
[install command]
[check command]
[test command]
[build command]
```

Document any required environment variables, local services, or generated files
near the command that needs them.

## Workflow

- Inspect the current files before proposing or editing code.
- Keep changes scoped to the requested task.
- Prefer existing project patterns over new abstractions.
- Do not rewrite stable files unless their behavior or contract changes.
- Preserve user changes that are unrelated to the task.
- Link to the owning standard instead of duplicating policy.

## Code Expectations

- Follow the project language and tooling choices.
- Keep TypeScript strict and fix unsafe types at the source.
- Validate untrusted runtime boundaries before using the value.
- Keep comments focused on constraints, tradeoffs, side effects, and failure
  modes.
- Document public APIs when names and types do not fully explain correct use.

## Checks

Before finishing, run the smallest checks that prove the change:

```sh
[format or lint command]
[typecheck command]
[test command]
```

If a check cannot be run, say exactly which check was skipped and why.

## Final Response

Final responses should include:

- What changed.
- What was verified.
- Any files or follow-ups the maintainer should review.
- Any checks that were not run.

## Related Standards

- [Tooling](../../standards/tooling.md)
- [Code documentation](../../standards/code/documentation.md)
- [JavaScript](../../standards/code/javascript.md)
- [TypeScript](../../standards/code/typescript.md)
- [JSDoc](../../standards/code/jsdoc.md)
- [Commits](../../standards/workflow/commits.md)
- [Pull requests](../../standards/workflow/pull-requests.md)
