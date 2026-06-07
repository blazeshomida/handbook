# JavaScript Standard

## Purpose

Use this standard when writing runtime-agnostic JavaScript patterns for JavaScript and TypeScript projects.

It covers naming, modules, functions, async code, errors, nullish values, object and array patterns, iteration, general comments, file organization, and runtime-agnostic examples. It does not own TypeScript-only type rules, JSDoc mechanics, formatter or linter choices, Node-specific APIs, Deno-specific APIs, or React-specific conventions.

Use [the tooling standard](../tooling.md) for formatter, linter, runtime, and toolchain choices.

## Naming

Names should reveal purpose, unit, state, or outcome.

Use domain terms consistently. Prefer names that describe what a value means in the current boundary.

```ts
// Avoid:
const data = await load();
const item = rows[0];

// Do:
const customerRows = await loadCustomerRows();
const firstRejectedRow = rows[0];
```

Short names are fine when local context supplies the meaning.

```ts
for (const row of rows) {
  validateImportRow(row);
}
```

Use longer names when several similar values are in scope.

```ts
const sourceAccountId = transfer.sourceAccountId;
const targetAccountId = transfer.targetAccountId;
```

Avoid vague nouns such as `data`, `item`, `thing`, `helper`, `manager`, and `util` unless the surrounding domain gives them a precise meaning.

## Modules, Imports, And Exports

Keep module boundaries clear.

- Put related behavior near the data and decisions it depends on.
- Export the smallest useful public surface.
- Prefer named exports for shared library code.
- Avoid default exports when a module exposes several related values.
- Keep side effects visible. Do not hide I/O, registration, or global mutation in import-only modules unless the file is explicitly an entrypoint.
- Avoid circular imports.
- Use `mod` for Deno barrel files and `index` for barrel files in other JavaScript runtimes.

Do not repeat module or namespace context in every exported name.

```ts
// Avoid:
export function parseCsvInput(input) {
  // ...
}

// Do:
export function parse(input) {
  // ...
}
```

Import from public module paths. Avoid deep internal dependency paths unless the dependency explicitly supports them.

## Functions

Functions should have one clear responsibility.

- Prefer straightforward functions over clever compression.
- Split functions when parsing, validation, I/O, mapping, and formatting become separate concerns.
- Add an abstraction only when it removes real duplication, names a stable concept, isolates a boundary, or makes testing clearer.
- Make mutation and side effects clear from the function name, parameters, return value, or nearby documentation.

Use options objects when a function would otherwise accumulate optional parameters.

```ts
// Avoid:
resolveAddress(hostname, "ipv4", 5000);

// Do:
resolveAddress(hostname, { family: "ipv4", timeoutMs: 5000 });
```

For composable library helpers, prefer data-last APIs when the function is meant to work in a `pipe` or `flow` style. Keep direct app code straightforward, and do not force pipeline style when a normal function call is clearer.

```ts
// Do for pipeable helpers:
const keepActive = filterBy((user) => user.active);
const activeUsers = keepActive(users);

// Also fine when composition is not the goal:
const activeUsers = filterActiveUsers(users);
```

Prefer early returns for invalid, empty, or unsupported cases when they make the main path easier to follow.

```ts
if (!requestBody) {
  return invalidRequest("Missing request body");
}

if (!canImportRows(user)) {
  return forbidden("User cannot import rows");
}

return importRows(requestBody);
```

## Async Code

Async code should make ordering, concurrency, and failure behavior visible.

- `await` promises when later work depends on their result or failure.
- Use `Promise.all` only when operations can safely run concurrently.
- Use sequential loops when order, rate limits, transactions, or shared state matter.
- Avoid floating promises unless the fire-and-forget behavior is intentional and documented.
- Preserve useful context when async work fails.

```ts
// Avoid:
rows.map(async (row) => importRow(row));

// Do:
await Promise.all(rows.map((row) => importRow(row)));
```

```ts
// Do when order or rate limits matter:
for (const row of rows) {
  await importRow(row);
}
```

## Errors

Make failure modes visible and consistent within a boundary.

- Do not catch unknown errors and return success, empty results, or unrelated fallback values.
- Preserve useful context when wrapping or rethrowing errors.
- Return or throw errors consistently inside the same layer.
- User-facing error messages should name the failed action and useful state.

```ts
// Avoid:
try {
  return parseRows(input);
} catch {
  return [];
}

// Do:
try {
  return parseRows(input);
} catch (error) {
  throw new Error(`Cannot parse import rows from ${fileName}`, { cause: error });
}
```

Use [the code documentation standard](documentation.md) when callers need to understand expected failure conditions, retry behavior, or partial effects.

## Null And Undefined

Use nullish values deliberately.

- Use `undefined` for omitted optional values.
- Use `null` when absence is an intentional domain value.
- Do not use falsy checks when `0`, `false`, or `""` are valid values.
- Prefer `??` when only `null` and `undefined` should fall back.
- Prefer optional chaining for safe property access, not for hiding invalid state.

```ts
// Avoid:
const retryCount = options.retryCount || 3;

// Do:
const retryCount = options.retryCount ?? 3;
```

When a value must exist, validate it near the boundary instead of passing `null` or `undefined` deeper into the system.

## Objects And Arrays

Use object and array patterns that keep data shape changes visible.

- Prefer object literals for named fields and options.
- Prefer arrays for ordered collections, not loosely related tuples.
- Avoid mutating inputs unless mutation is part of the function contract.
- Create new objects or arrays when that makes ownership clearer.
- Normalize external data near the boundary before treating it as internal data.

```ts
const normalizedUser = {
  id: rawUser.id,
  email: rawUser.email.trim().toLowerCase(),
  displayName: rawUser.name ?? "Unknown user",
};
```

Use destructuring when it improves clarity. Avoid destructuring so much at once that the source of values becomes hard to see.

## Iteration

Choose iteration based on the work being done.

- Use `map` to transform every item.
- Use `filter` to keep matching items.
- Use `find` to return the first matching item.
- Use `some` or `every` for boolean checks.
- Use `reduce` only when it clearly names the accumulated value.
- Use `for...of` when the loop has branching, async steps, mutation, or multiple effects.

```ts
// Avoid:
const activeUsers = users.reduce((acc, user) => {
  if (user.active) acc.push(user);
  return acc;
}, []);

// Do:
const activeUsers = users.filter((user) => user.active);
```

Avoid chaining many transformations when naming intermediate values would make the behavior easier to review.

## Comments

Comments should explain context the code cannot make obvious.

Use comments for non-obvious intent, external constraints, security or data safety requirements, performance tradeoffs, compatibility requirements, and TODOs with ownership or a clear trigger.

```ts
// Avoid:
// Loop through users and check status.
for (const user of users) {
  await checkStatus(user);
}

// Do:
// The billing API allows one status lookup per customer per second.
for (const user of users) {
  await limit(() => checkStatus(user));
}
```

Follow [the code documentation standard](documentation.md) for broader documentation judgment. Keep JSDoc mechanics in [the JSDoc standard](jsdoc.md).

## File Organization

A file should make its role easy to scan.

- Put imports first.
- Keep top-level constants close to the behavior that uses them.
- Put the primary exported behavior where readers can find it quickly.
- Keep private helpers near their callers unless they are shared by several functions in the file.
- Split a file when unrelated responsibilities make the main behavior hard to follow.

Do not split files only to create abstraction. Split when responsibilities, ownership, or reuse boundaries are real.
