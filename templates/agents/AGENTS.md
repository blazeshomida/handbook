# Agent Instructions

## Project Overview

<!-- Replace this with the project purpose, product surface, and important ownership boundaries. -->

This repository follows Blaze's engineering handbook for reusable coding, documentation, workflow, and release standards.

Use this file for repository-specific instructions. Use the handbook as the canonical source for reusable standards unless this repository explicitly overrides them.

## Handbook References

Primary handbook:

* [Handbook](https://github.com/blazeshomida/handbook)
* [Standards](https://github.com/blazeshomida/handbook/tree/main/standards)
* [Templates](https://github.com/blazeshomida/handbook/tree/main/templates)

Use the most relevant standard before making broad changes:

* [TypeScript Standard](https://github.com/blazeshomida/handbook/blob/main/standards/code/typescript.md)
* [JavaScript Standard](https://github.com/blazeshomida/handbook/blob/main/standards/code/javascript.md)
* [Code Documentation Standard](https://github.com/blazeshomida/handbook/blob/main/standards/code/documentation.md)
* [Tooling Standard](https://github.com/blazeshomida/handbook/blob/main/standards/tooling.md)
* [Commit Standard](https://github.com/blazeshomida/handbook/blob/main/standards/workflow/commits.md)
* [Pull Request Standard](https://github.com/blazeshomida/handbook/blob/main/standards/workflow/pull-requests.md)
* [Changesets Standard](https://github.com/blazeshomida/handbook/blob/main/standards/workflow/changesets.md)
* [Release Standard](https://github.com/blazeshomida/handbook/blob/main/standards/workflow/releases.md)

If internet access is unavailable, follow the critical rules in this file.

## Commands

<!-- Replace or remove commands that do not apply. Prefer exact project commands. -->

Prefer the package manager already used by the repository. Do not switch package managers unless explicitly requested.

```sh
# Install dependencies
<install command>

# Run checks
<check command>

# Run tests
<test command>

# Build
<build command>

# Run the app or package locally
<dev command>
```

## Workflow

* Inspect the current repo state before changing files.
* Read the relevant files before implementing changes.
* Keep edits scoped to the requested task.
* Prefer existing project patterns over new abstractions.
* Do not rewrite existing files unless the requested behavior requires it.
* Do not revert user changes unless explicitly asked.
* Do not introduce unrelated formatting changes.
* Do not rename files, move modules, or change public APIs unless the task requires it.
* Check package scripts before inventing commands.
* Check existing tests before adding new test structure.
* Check public exports before adding or removing exports.
* Summarize changed files, verification, and any remaining risk before handing work back.

## Type Safety

Follow the [TypeScript Standard](https://github.com/blazeshomida/handbook/blob/main/standards/code/typescript.md).

* Prefer TypeScript for application and library code.
* Keep untrusted values at boundaries until they are validated or narrowed.
* Avoid `any`.
* Do not use `as any`.
* Prefer `unknown` at untrusted boundaries.
* Fix types at the source instead of casting at call sites.
* Keep unsafe casts local, narrow, and justified by nearby runtime checks.
* Prefer `satisfies` for config objects and lookup maps when useful.
* Use type-only imports and exports for type-only dependencies.
* Keep public API types named, readable, and stable.
* Prefer discriminated unions for state and result modeling.
* Prefer explicit return types for exported functions.

## Code Style

* Follow existing project style first.
* Prefer small, focused modules.
* Prefer explicit public exports.
* Avoid accidental root exports.
* Prefer functional utilities unless a class is clearly the better model.
* Keep names descriptive and consistent with nearby code.
* Keep examples practical and minimal.
* Avoid clever abstractions that do not reduce real duplication.
* Omit comments that repeat the code.

## Public API Changes

When changing public API behavior:

* Preserve backwards compatibility unless a breaking change is requested.
* Update docs and examples alongside code.
* Add or update tests for user-visible behavior.
* Check export boundaries.
* Mention the API impact in the final response.

For packages, avoid adding new root exports unless explicitly requested or already established by the package design.

## Documentation

Follow the [Code Documentation Standard](https://github.com/blazeshomida/handbook/blob/main/standards/code/documentation.md).

* Document public API behavior, constraints, side effects, and failure modes when names and signatures are not enough.
* Document runtime requirements and non-obvious examples when useful.
* Omit comments that repeat the code.
* Keep examples minimal, realistic, and focused on the documented behavior.

## Testing And Checks

Follow the [Tooling Standard](https://github.com/blazeshomida/handbook/blob/main/standards/tooling.md).

* Run the smallest reliable checks that cover the change.
* Run broader checks when touching shared behavior, public APIs, build config, or release paths.
* If a check is not run, say so and explain why.
* If tests do not exist for the touched area, say that clearly and explain what was verified instead.

Examples:

* Type-only change: run type-check or the repo's check command.
* Utility behavior change: run related unit tests.
* UI behavior change: run relevant tests and, when possible, build.
* Build config change: run build.
* Public package change: run tests, build, and type-check.

## Dependencies

Before adding a dependency:

* Check whether the repo already has an equivalent utility.
* Prefer small, well-maintained dependencies.
* Avoid adding dependencies for trivial logic.
* Explain why the dependency is needed.
* Update lockfiles consistently.

Do not switch package managers.

## Generated Files

Do not manually edit generated files unless the repository explicitly requires it.

Common generated files may include:

* route trees
* generated API clients
* build output
* coverage output
* declaration output
* lockfile-only changes from unrelated installs

If generated files need updating, use the repository's generation command.

## Commits

Follow the [Commit Standard](https://github.com/blazeshomida/handbook/blob/main/standards/workflow/commits.md).

Before suggesting a commit message:

* Identify the primary change.
* Use the smallest useful scope.
* Use one primary intent.
* Avoid mixing unrelated changes.
* Mention breaking changes only when behavior or API compatibility actually breaks.

Do not create commits unless explicitly asked.

## Pull Requests

Follow the [Pull Request Standard](https://github.com/blazeshomida/handbook/blob/main/standards/workflow/pull-requests.md).

A good PR summary should include:

* what changed
* why it changed
* how it was verified
* risks, tradeoffs, or follow-up work

For UI changes, include screenshots or describe the visual difference.

## Safety And Secrets

* Do not print secrets, tokens, private keys, or credentials.
* Do not commit `.env` files unless the repo intentionally tracks an example file.
* Prefer `.env.example` for documented environment variables.
* Redact sensitive values in logs and responses.

## Final Response

When work is complete, include:

* What changed.
* What was verified.
* Anything not run or not completed.
* Any follow-up that is required before merge or release.

Keep the response concise and specific to the completed work.
