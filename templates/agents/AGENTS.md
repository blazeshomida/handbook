# Agent Instructions

## Project Overview

<!-- Replace this with the project purpose, product surface, and important ownership boundaries. -->

## Commands

<!-- Replace or remove commands that do not apply. Prefer exact project commands. -->

```sh
# Install dependencies
<install command>

# Run checks
<check command>

# Run tests
<test command>

# Run the app or package locally
<dev command>
```

## Workflow

- Inspect the current repo state before changing files.
- Keep edits scoped to the requested task.
- Prefer existing project patterns over new abstractions.
- Do not rewrite existing files unless the requested behavior requires it.
- Do not revert user changes unless explicitly asked.
- Summarize changed files, verification, and any remaining risk before handing
  work back.

Reference: [Commit Standard](HANDBOOK_URL/standards/workflow/commits.md)

## Type Safety

- Prefer TypeScript for application and library code.
- Keep untrusted values at boundaries until they are validated or narrowed.
- Avoid `any`; fix types at the source when possible.
- Keep unsafe casts local and justified by nearby runtime checks.

Reference: [TypeScript Standard](HANDBOOK_URL/standards/code/typescript.md)

## Documentation

- Document public API behavior, constraints, side effects, and failure modes
  when names and signatures are not enough.
- Omit comments that repeat the code.
- Keep examples minimal, realistic, and focused on the documented behavior.

Reference: [Code Documentation Standard](HANDBOOK_URL/standards/code/documentation.md)

## Testing And Checks

- Run the smallest reliable checks that cover the change.
- Run broader checks when touching shared behavior, public APIs, or release
  paths.
- If a check is not run, say so and explain why.

Reference: [Tooling Standard](HANDBOOK_URL/standards/tooling.md)

## Final Response

When work is complete, include:

- What changed.
- What was verified.
- Anything not run or not completed.
- Any follow-up that is required before merge or release.
