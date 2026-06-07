# Tooling Standard

## Purpose

Use this standard when choosing project tooling, formatting, linting, testing, build tools, and common platform dependencies.

It covers default tool preferences and tooling boundaries. It does not define JavaScript style, TypeScript type rules, JSDoc tags, commit format, or release process.

## Default Language And Runtime

- Prefer TypeScript for application and library code.
- Use Vite+ for non-Deno JavaScript and TypeScript projects.
- Use Deno tooling for Deno projects.
- Do not mix Node and Deno tooling without a concrete project reason.

## Formatting And Linting

- Use Vite+ `vp check` for non-Deno projects so formatting, linting, and type checks run through one project entry point.
- Use Deno's formatter and linter for Deno projects instead of adding Vite+ by default.
- Let automated tooling own mechanical style. Code standards should own judgment that tooling cannot fully enforce.

This handbook repo uses Vite+ static checks for automated formatting, linting, and type checks. Do not add an application entrypoint or build output solely to make app-oriented commands pass. Do not add Deno solely to format Markdown.

## Frameworks And UI

- Prefer React with React Compiler for React applications.
- Prefer TanStack Start for React application framework work.
- Use Next.js selectively when its platform, routing, deployment, or ecosystem fit is the concrete reason.
- Use SolidJS selectively when fine-grained reactivity is the concrete reason.
- Prefer Tailwind CSS for styling.
- Prefer shadcn/ui with Base UI components, Luma style, a zinc base, and Tabler icons.
- Use shadcn presets when a project should change the visual baseline.
- Use Motion for animation.
- Do not choose Astro solely for content collections when the chosen app stack already has a suitable content collections path.

## State, Data, And Forms

- Prefer TanStack Query for async server state.
- Prefer TanStack Store for client state.
- Prefer TanStack Form for forms.
- Use XState only when state complexity justifies it. For small flows, still model states and transitions explicitly without adding XState by default.

## Backend, Auth, And Validation

- Prefer Convex for backend and database needs.
- Prefer Clerk for authentication.
- Prefer t3-env for environment variable validation.
- Prefer Valibot for schema validation.

## Testing, Builds, And Release Tooling

- Prefer Vitest for tests unless the project is Deno-based.
- Use Deno's test tooling in Deno projects.
- Use Vite+ `vp build` for non-Deno app builds.
- Use Vite+ `vp pack` for non-Deno publishable libraries and standalone executables.
- Put package-build configuration in the `pack` block in `vite.config.ts`; do not add a separate `tsdown.config.ts` when Vite+ owns the workflow.
- Prefer git-cliff for changelog generation from Git history.
- Prefer Changesets for package versioning and release intent when a package release flow needs per-change declarations.
- If a project uses both Changesets and git-cliff, keep their roles separate: Changesets owns package versioning and release intent, and git-cliff owns Git-history changelog output.
- Do not configure both Changesets and git-cliff to write the same changelog output for the same release flow.
- Use Turbo only when workspace orchestration is needed.
- Use CodeMirror for editor surfaces.
- Use Shiki for syntax highlighting.

## Dependency Policy

- Start with the default tools in this standard.
- Add a dependency when it solves a real project need better than local code or existing tooling.
- Avoid adding overlapping tools for the same job without a clear boundary.
- Prefer project-specific overrides when the project type, deployment target, or runtime makes the default tool a poor fit.
- Add specialized tools only when Vite+ or Deno cannot cover a documented project requirement.
- Document meaningful deviations near the project tooling config or setup docs.

## Official References

- [Vite+ guide](https://viteplus.dev/guide/) for the unified `vp` workflow.
- [Vite+ install](https://viteplus.dev/guide/install) for package-management commands across package managers.
- [Vite+ check](https://viteplus.dev/guide/check) for the integrated format, lint, and type-check command.
- [Vite+ lint](https://viteplus.dev/guide/lint) for Vite+'s lint command.
- [Vite+ format](https://viteplus.dev/guide/fmt) for Vite+'s formatter command.
- [Vite+ pack](https://viteplus.dev/guide/pack) for library and standalone executable builds.
- [Vite build guide](https://vite.dev/guide/build) for app builds and library mode boundaries.
- [Deno linting and formatting](https://docs.deno.com/runtime/fundamentals/linting_and_formatting/) for Deno-native `deno fmt` and `deno lint`.
