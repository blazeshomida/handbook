# TypeScript Standard

## Purpose

Use this standard for TypeScript-specific rules that extend
[the JavaScript standard](javascript.md).

Use this standard when writing TypeScript APIs, boundary types, and type-driven
control flow. It covers strict TypeScript, avoiding `any`, avoiding unsafe
casts, fixing types at the source, `unknown` at boundaries, runtime validation
at boundaries, `satisfies`, type imports and exports, generics, discriminated
unions, and public API typing. It does not own general JavaScript style,
formatter or linter choices, React-specific conventions, JSDoc mechanics, or
long TypeScript tutorials.

Use [the tooling standard](../tooling.md) for TypeScript tooling choices and
[the code documentation standard](documentation.md) for public API documentation
judgment.

## Strict TypeScript

Prefer strict TypeScript for application and library code.

Strictness should make invalid states visible while code is still local enough
to fix. Do not weaken project-level type checking to work around one hard type.
Fix the model, narrow the input, or isolate the boundary.

## Public API Typing

Public APIs should have named, readable, and stable types.

Use explicit types for:

- Exported functions.
- Exported classes and public fields.
- Exported constants when the inferred type is not obvious.
- Public options and result objects.
- Complex values that appear in generated declarations or API docs.

```ts
// Avoid:
export function createImportJob(file: File) {
  return {
    id: crypto.randomUUID(),
    file,
    createdAt: new Date(),
  };
}

// Do:
export interface ImportJob {
  id: string;
  file: File;
  createdAt: Date;
}

export function createImportJob(file: File): ImportJob {
  return {
    id: crypto.randomUUID(),
    file,
    createdAt: new Date(),
  };
}
```

Let simple local implementation details infer when the inferred type is obvious.
Do not annotate every local variable by default.

## Avoid `any`

Avoid `any` because it removes type checking from the code that touches it.

Use better alternatives:

- Use `unknown` for untrusted inputs.
- Use generic parameters when the caller should keep a type relationship.
- Use named unions for known variants.
- Fix missing library or application types at the source.

```ts
// Avoid:
function readPayload(value: any): Payload {
  return value;
}

// Do:
function readPayload(value: unknown): Payload {
  if (!isPayload(value)) {
    throw new TypeError("Cannot read payload: value is not a payload");
  }

  return value;
}
```

If `any` is unavoidable at a boundary, keep it local, convert it immediately,
and do not let it spread into internal code.

## Unknown And Runtime Boundaries

Use `unknown` at untrusted boundaries, then validate or narrow before use.

Untrusted boundaries include request bodies, files, environment variables,
database rows, third-party API responses, user input, and message payloads.

```ts
export function parseWebhook(value: unknown): WebhookEvent {
  if (!isWebhookEvent(value)) {
    throw new TypeError("Cannot parse webhook: invalid payload");
  }

  return value;
}
```

Runtime validation belongs near the boundary. After validation, use names that
make the state clear, such as `rawWebhookPayload` and `validatedWebhookEvent`.

Use [the tooling standard](../tooling.md) for validation library preferences.

## Unsafe Casts

Avoid casts that tell TypeScript to trust code the runtime has not proven.

```ts
// Avoid:
const payload = value as Payload;

// Do:
const payload = parsePayload(value);
```

Do not stack casts such as `value as unknown as Target`. That usually means the
source type, boundary parser, or public API type needs to be fixed.

Use a cast only when the runtime condition is already guaranteed and TypeScript
cannot express it. Keep the cast close to the check that justifies it.

## Fix Types At The Source

Fix incorrect types where they originate.

- Update the function signature instead of casting every caller.
- Narrow boundary values before passing them deeper.
- Add a missing return type to the public function instead of relying on inferred
  exported shapes.
- Model variants directly instead of using broad strings or optional fields.

Avoid local casts that make one call site compile while leaving the wrong type
available elsewhere.

## `satisfies`

Use `satisfies` when a value must conform to a shape without losing its specific
literal information.

```ts
const statusLabels = {
  queued: "Queued",
  running: "Running",
  failed: "Failed",
} satisfies Record<ImportStatus, string>;
```

Prefer `satisfies` for configuration maps, lookup tables, and object literals
where both completeness checking and precise value inference matter.

Do not use `satisfies` to hide an overly broad or incorrect target type.

## Type Imports And Exports

Use type-only imports and exports for values used only as types.

```ts
import type { ImportJob } from "./jobs";

export type { ImportJob } from "./jobs";
```

Keep value imports and type imports distinct when that improves runtime clarity.
Do not rely on a type import to create a runtime dependency.

Export public types deliberately. Avoid `export *` from internal modules when it
makes accidental internals part of the public contract.

## Generics

Use generics to preserve a real relationship between inputs and outputs.

```ts
function first<T>(values: readonly T[]): T | undefined {
  return values[0];
}
```

Avoid generics that only make a function look flexible without improving caller
safety.

```ts
// Avoid:
function parseJson<T>(value: string): T {
  return JSON.parse(value) as T;
}

// Do:
function parseJson(value: string): unknown {
  return JSON.parse(value);
}
```

Name generic parameters after their role when a single-letter name is not clear.

```ts
function mapById<Item extends { id: string }>(
  items: readonly Item[],
): Map<string, Item> {
  return new Map(items.map((item) => [item.id, item]));
}
```

Keep public generic types readable. Hide necessary complexity behind named type
aliases.

## Discriminated Unions

Use discriminated unions for meaningful variants.

```ts
type ImportResult =
  | { ok: true; imported: number }
  | { ok: false; reason: "missing_header"; header: string }
  | { ok: false; reason: "invalid_row"; row: number };
```

Prefer a stable discriminator such as `kind`, `type`, `status`, or `ok`.

Use discriminated unions instead of parallel optional fields when each variant
has different required data.

```ts
// Avoid:
interface ImportResult {
  ok: boolean;
  imported?: number;
  reason?: string;
}
```

When handling a union, check the discriminator first so narrowing is obvious.

## Interfaces And Type Aliases

Prefer `interface` for public object shapes that may be implemented, extended,
or composed with other object shapes.

```ts
export interface ImportOptions {
  dryRun?: boolean;
  batchSize?: number;
}
```

Use `type` for unions, primitives, tuples, mapped types, and aliases that are
not object extension points.

```ts
type ImportStatus = "queued" | "running" | "failed" | "completed";
type CsvRow = readonly [name: string, email: string];
```

Keep complex public types named and readable. A type that is hard to explain is
usually hard to use correctly.

## Review Checklist

Use this checklist when reviewing TypeScript-specific code:

- Is strict type checking preserved?
- Is `any` avoided or contained at a boundary?
- Are untrusted values typed as `unknown` before validation?
- Are runtime boundary values validated before internal use?
- Are unsafe casts replaced with source type fixes or boundary parsers?
- Are exported APIs typed explicitly when needed for declarations or docs?
- Are public parameter and return shapes named?
- Is `satisfies` used where shape conformance and literal inference both matter?
- Are type-only imports and exports used for type-only dependencies?
- Do generics preserve real input and output relationships?
- Are meaningful variants modeled as discriminated unions?
