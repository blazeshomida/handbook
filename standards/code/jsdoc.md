# JSDoc Standard

## Purpose

Use JSDoc to make public JavaScript and TypeScript APIs easier to understand in
source, editor hovers, and generated API documentation.

This file owns approved and discouraged JSDoc tags, formatting conventions,
examples, public API usage, deprecations, internal API usage, and links. It does
not own general documentation philosophy, general JavaScript style, TypeScript
type-safety rules, or formatter and linter choices. Follow the active file
contract in [PLAN.md](../../PLAN.md).

Use [the code documentation standard](documentation.md) for documentation
judgment, [the JavaScript standard](javascript.md) for JavaScript patterns, and
[the TypeScript standard](typescript.md) for TypeScript type rules.

## Documentation Scope

Document the public surface area when names and types do not fully communicate
behavior.

The public surface includes:

- Exported functions.
- Exported classes.
- Exported types and interfaces.
- Exported constants when behavior, units, or meaning are not obvious.
- Public modules and package entrypoints.

Do not require JSDoc for private helpers, local variables, or implementation
details. Add internal comments only when they prevent a likely misunderstanding.

Document re-exported symbols at the source. Do not duplicate API documentation
across barrel files unless the re-export changes the public meaning.

```ts
// Avoid:
/**
 * Adds two numbers.
 */
export { add } from "./math";

// Do:
export { add } from "./math";
```

## Summary Lines

Every public JSDoc block must start with a short behavior summary.

The summary should say what the API does, not how it is implemented. Avoid
summaries that only repeat the symbol name.

```ts
// Avoid:
/**
 * parseDuration function.
 */
export function parseDuration(value: string): number {
  // ...
}

// Do:
/**
 * Parses a duration string into milliseconds.
 */
export function parseDuration(value: string): number {
  // ...
}
```

Add paragraphs after the summary only when they explain constraints,
guarantees, tradeoffs, examples, or failure modes that do not fit in one
sentence.

## Document Behavior, Not Obvious Types

Use JSDoc for meaning the type system and names do not fully communicate.

Document these details when relevant:

- Input constraints.
- Units.
- Accepted formats.
- Defaults.
- Side effects and mutation.
- I/O.
- Async timing.
- Ordering guarantees.
- Caching behavior.
- Performance characteristics that affect usage.

Do not restate obvious parameter names, TypeScript types, or implementation
steps.

```ts
// Avoid:
/**
 * Formats bytes.
 *
 * @param bytes The bytes number.
 * @returns A string.
 */
export function formatBytes(bytes: number): string {
  // ...
}

// Do:
/**
 * Formats a byte count using binary units.
 *
 * Values are rounded to one decimal place after conversion.
 */
export function formatBytes(bytes: number): string {
  // ...
}
```

## Approved Tags

Use tags only when they add information.

| Tag | Use it for |
| --- | --- |
| `@param` | Constraints, units, formats, or non-obvious parameter meaning. |
| `@returns` | Return semantics that need explanation. |
| `@throws` | Caller-visible thrown errors and conditions. |
| `@example` | Minimal usage examples. |
| `@deprecated` | Deprecation reason, replacement, and removal timing when known. |
| `@see` | Related public APIs that deserve their own line. |
| `@module` | Public module or package entrypoint documentation. |

Use project-specific tags only when the documentation tool supports them and the
tag adds information. Prefer prose over uncommon tags for ordinary extended
explanation.

## Discouraged Tags

Avoid tags that repeat TypeScript syntax or add noise.

- Do not use Closure-style type annotations in TypeScript files, such as
  `@param {string} id`.
- Do not use `@param` when it only repeats the parameter name.
- Do not use `@returns` when it only repeats the return type.
- Do not use tags only to force TypeScript inference.
- Do not use project-specific tags that generated docs ignore.
- Do not use examples as the only place where required constraints or failure
  modes are explained.

In JavaScript files, JSDoc type annotations may be useful because JavaScript does
not have TypeScript syntax. Keep those annotations consistent with the same
behavior rules in this standard.

## Parameters

Use `@param` only when a parameter needs more than its name and type.

Document a parameter when it has constraints, units, accepted formats,
protocol-specific meaning, or non-obvious behavior.

```ts
// Avoid:
/**
 * Finds a user.
 *
 * @param id The id.
 */
export function findUser(id: string): User | null {
  // ...
}

// Do:
/**
 * Finds a user by public user id.
 *
 * @param id Must use the `usr_` prefix from the public API.
 */
export function findUser(id: string): User | null {
  // ...
}
```

## Returns And Failures

Use `@returns` when return semantics need explanation.

Document return values when the API returns:

- `null` or `undefined`.
- A union where each variant has meaning.
- A tuple or collection with ordering guarantees.
- A value with units.
- A value with a non-obvious default or fallback.

```ts
/**
 * Looks up a user by id.
 *
 * @returns The user, or `null` when no user exists for the id.
 */
export function findUser(id: string): User | null {
  // ...
}
```

Use `@throws` when a function throws for expected caller-visible conditions.
Include the condition, not only the error class.

```ts
/**
 * Parses JSON configuration.
 *
 * @throws When the value does not contain valid JSON.
 */
export function parseConfig(value: string): Config {
  // ...
}
```

Also document failure-like results that do not throw, such as `null`,
`undefined`, result variants, rejected promises, empty collections with special
meaning, and partial success.

## Examples

Examples are part of the public API contract.

Use `@example` when public API usage is not trivial. Examples should be minimal,
realistic, copyable, runnable when possible, and TypeScript-valid.

Every `@example` must include a short title. Use `Basic usage.` when there is
only one obvious case.

````ts
/**
 * Formats bytes as a human-readable string.
 *
 * @example Basic usage.
 * ```ts
 * import { formatBytes } from "./format-bytes";
 *
 * formatBytes(1536); // "1.5 KiB"
 * ```
 */
export function formatBytes(bytes: number): string {
  // ...
}
````

Prefer one strong example over several redundant examples. Add more than one
example only when the examples show meaningfully different use cases.

Include imports and setup values when they are short enough to keep the example
copyable.

````ts
/**
 * Creates an import job.
 *
 * @example Create a dry-run import.
 * ```ts
 * import { createImportJob } from "./imports";
 *
 * const file = new File(["name,email\nAda,ada@example.com"], "customers.csv");
 * const job = createImportJob(file, { dryRun: true });
 * ```
 */
export function createImportJob(file: File, options?: ImportOptions): ImportJob {
  // ...
}
````

Avoid examples that introduce unrelated framework, runtime, or tooling policy.

## Deprecations

Use `@deprecated` when a public API should no longer be used.

Deprecation comments should explain:

- What is deprecated.
- What to use instead.
- The version, date, or condition for removal when known.

```ts
/**
 * Creates a user session.
 *
 * @deprecated Use `createSession` instead. This wrapper will be removed in
 * version 3.0.0.
 */
export function createUserSession(user: User): Session {
  // ...
}
```

Do not deprecate an API without naming a replacement unless there is no
replacement.

## Links

Use links to make related public APIs navigable.

Use inline links in prose and `@see` for related APIs that deserve their own
line.

Common inline link forms:

- `{@link Symbol}` for ordinary links.
- `{@linkcode Symbol}` when the linked text should render as code.
- `{@linkplain Symbol}` when the linked text should render as plain text.

```ts
/**
 * Creates an iterator over numbers.
 *
 * Use {@link rangeClosed} when the end value should be included.
 *
 * @see {@link rangeClosed}
 */
export function range(start: number, end: number): Iterable<number> {
  // ...
}
```

Link to public symbols, not private helpers that generated docs cannot expose.

## Module Documentation

Public modules and package entrypoints should have module-level documentation
when generated docs benefit from context.

Module documentation should explain:

- What the module provides.
- How the main exports relate to each other.
- The smallest useful happy-path example.

````ts
/**
 * Iteration utilities and iterator adapters.
 *
 * @module iter
 *
 * @example Basic usage.
 * ```ts
 * import { map, range } from "./iter";
 *
 * for (const value of map(range(0, 3), (n) => n * 2)) {
 *   console.log(value);
 * }
 * ```
 */
````

Module-level documentation should reduce repeated per-symbol explanation. It
should not become a README inside a source file.

## Internal APIs

Do not add JSDoc to private helpers by default.

Use internal JSDoc only when a non-public API is still shared enough that future
readers need generated or hover documentation. Prefer ordinary comments for
local implementation constraints.

Internal documentation should follow the same behavior-first rules as public
documentation. Do not use internal JSDoc to preserve history or describe obvious
control flow.

## Formatting

Format JSDoc blocks consistently:

- Start with a one-sentence behavior summary.
- Put a blank line between the summary, longer prose, and tag groups.
- Put tags after prose.
- Keep tag descriptions concise and sentence-like.
- Use code spans for literal values, options, and symbols.
- Keep examples fenced and titled.

Do not use JSDoc as a formatting workaround. Formatter and linter choices belong
in [the tooling standard](../tooling.md).

## Review Checklist

Use this checklist before merging public API documentation:

- Does each public JSDoc block start with a behavior summary?
- Are tags used only when they add information?
- Are TypeScript types not repeated in JSDoc tags?
- Are constraints, units, formats, defaults, and side effects documented?
- Are `null`, `undefined`, union variants, thrown errors, and rejections clear?
- Are examples minimal, realistic, copyable, and TypeScript-valid?
- Does every example have a useful title?
- Are deprecated APIs marked with `@deprecated` and a replacement when possible?
- Do links point to public APIs?
- Does internal JSDoc explain non-obvious shared behavior instead of local
  implementation steps?
