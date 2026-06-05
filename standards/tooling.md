# Tooling Standard

## Purpose

Use this standard to choose project tooling, formatting, linting, testing, build
tools, and common platform dependencies.

This file owns toolchain choices and boundaries. It does not define JavaScript
style, TypeScript type rules, JSDoc tags, commit format, or release process.
Follow the active file contract in [PLAN.md](../PLAN.md).

## Default Language And Runtime

- Prefer TypeScript for application and library code.
- Prefer pnpm for Node-based and package-managed projects unless Deno can cover
  the project cleanly.
- Use Deno tooling for Deno projects instead of layering Node tooling on top.
- Do not mix Node and Deno tooling without a concrete project reason.
- Prefer Vite-based tooling when the project does not require a framework-owned
  build pipeline.

## Formatting And Linting

- Prefer Biome for formatting and linting when it fits the project.
- Use Prettier where Biome does not fit, especially Markdown-only formatting in
  this handbook.
- Use ESLint only when the project needs rules Biome cannot cover.
- Let automated tooling own mechanical style. Code standards should own
  judgment that tooling cannot fully enforce.

For this handbook repo, use Prettier for Markdown formatting if package tooling
is added. Do not add Deno, Biome, or ESLint solely to format Markdown.

## Frameworks And UI

- Prefer React with React Compiler for React applications.
- Prefer TanStack Start for React application framework work.
- Use Next.js selectively when its platform, routing, deployment, or ecosystem
  fit is the concrete reason.
- Use SolidJS selectively when fine-grained reactivity is the concrete reason.
- Prefer Tailwind CSS for styling.
- Prefer shadcn/ui with Base UI components, Luma style, a zinc base, and Tabler
  icons.
- Use shadcn presets when a project should change the visual baseline.
- Use Motion for animation.
- Do not choose Astro solely for content collections when the chosen app stack
  already has a suitable content collections path.

## State, Data, And Forms

- Prefer TanStack Query for async server state.
- Prefer TanStack Store for client state.
- Prefer TanStack Form for forms.
- Use XState only when state complexity justifies it. For small flows, still
  model states and transitions explicitly without adding XState by default.

## Backend, Auth, And Validation

- Prefer Convex for backend and database needs.
- Prefer Clerk for authentication.
- Prefer t3-env for environment variable validation.
- Prefer Valibot for schema validation.

## Testing, Builds, And Release Tooling

- Prefer Vitest for tests unless the project is Deno-based.
- Use Deno's test tooling in Deno projects.
- Prefer tsdown for package builds.
- Prefer git-cliff for changelog generation from Git history.
- Prefer Changesets for package versioning and release intent when a package
  release flow needs per-change declarations.
- If a project uses both Changesets and git-cliff, keep their roles separate:
  Changesets owns package versioning and release intent, and git-cliff owns
  Git-history changelog output.
- Do not configure both Changesets and git-cliff to write the same changelog
  output for the same release flow.
- Use Turbo only when workspace orchestration is needed.
- Use CodeMirror for editor surfaces.
- Use Shiki for syntax highlighting.

## Dependency Policy

- Start with the default tools in this standard.
- Add a dependency when it solves a real project need better than local code or
  existing tooling.
- Avoid adding overlapping tools for the same job without a clear boundary.
- Prefer project-specific overrides when the project type, deployment target, or
  runtime makes the default tool a poor fit.
- Document meaningful deviations near the project tooling config or setup docs.
