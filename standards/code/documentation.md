# Code Documentation Standard

## Purpose

Code documentation helps a capable developer understand behavior, constraints,
tradeoffs, failures, and public contracts without reconstructing local history.

Use this standard for code comments, public API documentation, and examples that
live near code. This file owns general documentation judgment. It does not own
JSDoc tag syntax, JavaScript style, TypeScript type-safety rules, tooling
choices, or README writing. Follow the active file contract in
[PLAN.md](../../PLAN.md).

## Core Standard

Document what the reader cannot safely infer from the code.

Good code documentation is:

- Clear about behavior.
- Close to the code it explains.
- Specific about conditions, units, side effects, and failure modes.
- Focused on why the code behaves the way it does.
- Easy to update when behavior changes.

Do not document implementation steps that the code already states clearly.

## When To Document

Add documentation when it helps a future reader make a correct change, call an
API correctly, or understand a non-obvious constraint.

Document code when it exposes:

- Public API behavior.
- Input constraints, units, formats, or defaults.
- Side effects such as mutation, I/O, caching, scheduling, or persistence.
- Failure modes and retry behavior.
- Ordering, timing, consistency, or performance guarantees.
- Security, privacy, compatibility, or migration constraints.
- A tradeoff that was chosen over another plausible design.
- An example that prevents a likely misuse.

Prefer documentation at the boundary where the reader needs the information.
Document public behavior near public APIs. Document local tradeoffs near the
code that depends on the tradeoff.

## When Not To Comment

Omit comments that only repeat code, translate syntax into prose, or preserve
history that no longer affects the reader.

```ts
// Avoid:
// Increment the retry count.
retryCount += 1;
```

Prefer clearer names, smaller functions, or more direct control flow before
adding a comment to explain confusing code.

```ts
// Avoid:
// Check whether the user can import rows.
if (u.p.includes("import")) {
  // ...
}

// Do:
if (userPermissions.includes("import")) {
  // ...
}
```

Use a comment only when the important context cannot be made obvious through the
code shape itself.

## Public APIs

Public APIs should explain behavior that names and signatures do not fully
communicate.

For public functions, classes, types, constants, modules, and package entry
points, document the details that affect correct use:

- What the API does.
- When to use it.
- Required input conditions.
- Meaningful defaults.
- Units and accepted formats.
- Return semantics that are not obvious.
- Caller-visible errors.
- Side effects and mutation.
- Stability or compatibility constraints.

Do not require public documentation to restate obvious names or types. Keep
syntax-specific rules in the future JSDoc standard.

## Constraints And Tradeoffs

Document constraints when they limit valid usage or explain why obvious
alternatives are unsafe.

```ts
// Keep batches below 500 rows. Larger batches increase validation memory enough
// to fail imports from nested CSV exports.
const maxBatchSize = 500;
```

Document tradeoffs when the code intentionally chooses one imperfect option over
another.

```ts
// This cache is process-local. Cross-worker invalidation would add network I/O
// to every lookup, and stale reads are acceptable for this status badge.
const statusCache = new Map<string, Status>();
```

Avoid comments that only record the author's process or a past investigation.
Keep the current reason and consequence.

## Failure Documentation

Make expected failure behavior visible at the boundary where callers need it.

Document:

- What can fail.
- Whether the failure is expected or exceptional.
- What context the caller receives.
- Whether retrying can succeed.
- Whether the operation is partial, atomic, or reversible.

```ts
// Import writes valid rows before reporting rejected rows. Retrying the same
// file is safe because row ids are deduplicated before insert.
```

Do not hide failure behavior in examples alone. If callers must handle it, state
the rule directly.

## Examples

Use examples when behavior or expectations can be misunderstood.

Good examples are:

- Minimal enough to scan.
- Realistic enough to teach the pattern.
- Focused on the behavior being documented.
- Consistent with existing standards.

Prefer examples for public APIs, configuration-sensitive behavior, error
handling, and constraints with non-obvious inputs.

Avoid examples that introduce unrelated language style, framework choices, or
tooling policy.

## Links And Duplication

Use links instead of repeating rules owned by another standard.

- Link to [the tooling standard](../tooling.md) for formatter, linter, runtime,
  and toolchain choices.
- When they exist, link to the JavaScript and TypeScript standards for
  language-specific implementation style.
- When it exists, link to the JSDoc standard for tag syntax and API
  documentation mechanics.

Do not add a link when a short local rule is clearer than making the reader
leave the file.
