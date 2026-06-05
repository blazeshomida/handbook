# Handbook Build Plan

## Goal

Build a focused `handbook` repo that acts as the source of truth for reusable engineering standards and file-level templates.

The handbook should help humans and coding agents understand:

- how the handbook should be edited
- how tooling preferences should be selected
- how changes should be committed
- how code should be documented
- how JavaScript and TypeScript should be written
- how JSDoc should be used
- how pull requests should be prepared
- how versions and releases should work
- which reusable file templates should be copied into projects

## Scope

### Included

- standards
- file-level templates
- agent coordination instructions
- tooling preferences
- commit conventions
- code documentation standards
- JavaScript standards
- TypeScript standards
- JSDoc conventions
- pull request conventions
- versioning conventions
- release conventions

### Excluded

- project templates
- cookbook recipes
- long-form guides
- reusable config packages
- scaffolding CLI
- generated documentation site

Project templates are out of scope for this plan. Do not define their names, stacks, repo structure, or implementation details here. If project templates become active scope later, create a separate plan.

## Repo boundary

The `handbook` repo should contain standards and copyable file templates.

It should not become a repo of runnable project templates.

```txt
handbook/templates = copyable files
project templates = separate future scope
```

## Target structure

```txt
handbook/
├─ README.md
├─ AGENTS.md
├─ PLAN.md
├─ standards/
│  ├─ README.md
│  ├─ tooling.md
│  ├─ code/
│  │  ├─ documentation.md
│  │  ├─ javascript.md
│  │  ├─ typescript.md
│  │  └─ jsdoc.md
│  └─ workflow/
│     ├─ commits.md
│     ├─ pull-requests.md
│     ├─ versioning.md
│     └─ releases.md
└─ templates/
   ├─ README.md
   ├─ agents/
   │  └─ AGENTS.md
   ├─ github/
   │  └─ pull_request_template.md
   └─ releases/
      ├─ changeset.md
      └─ release-notes.md
```

## Core strategy

Use a controlled workflow to avoid constant rewrites:

```txt
contract → draft → light review → targeted edits → stable → final review → frozen
```

The goal is not to make every file perfect immediately. The goal is to make each file good enough, prevent drift, then do one deeper consistency pass near the end.

## File states

Track files using these states:

```txt
planned → contracted → drafted → light-reviewed → stable → final-reviewed → frozen
```

### State meanings

| State | Meaning |
| --- | --- |
| `planned` | The file is part of the plan, but no contract exists yet. |
| `contracted` | The file purpose, scope, dependencies, and out-of-scope items are defined. |
| `drafted` | The first full draft exists. |
| `light-reviewed` | The file has been checked for scope, duplication, contradictions, links, and example alignment. |
| `stable` | The file is good enough to build on. Avoid reopening it unless needed. |
| `final-reviewed` | The file has passed the final cross-handbook review. |
| `frozen` | The file is part of the initial finished handbook. |

### Reopening stable files

A stable file should only be reopened when:

- one of its dependencies changes
- final review finds an issue
- the file contract changes
- the owner explicitly requests a revision

## Anti-rewrite policy

Avoid repeated full rewrites.

Rules:

- Do not rewrite an entire file unless its contract changes.
- Prefer targeted edits over broad rewrites.
- Use links instead of repeating rules from another standard.
- If a dependency is missing, add a TODO instead of guessing.
- Examples must follow already-written standards.
- Final polish happens during integration review, not during every draft.
- Do not reopen stable files for minor style preferences unless the issue affects clarity, correctness, or consistency.

## Orchestration policy

This plan may be handed to Codex as the source of truth.

Codex should act as the parent/orchestrator agent.

### Parent/orchestrator responsibilities

The parent agent owns:

- milestone order
- file contracts
- accepted decisions
- actual edits
- applying accepted review findings
- moving files between states
- deciding when a milestone is complete
- stopping after the requested milestone or batch of milestones

### Subagent permission

When explicitly authorized by the kickoff prompt, the parent agent may spawn subagents only at review steps that explicitly allow subagents.

Subagents must be read-only unless the user explicitly grants write permission.

The parent agent must:

- wait for all subagents
- consolidate findings
- summarize findings before applying edits
- decide which findings are accepted
- apply targeted edits itself
- avoid letting subagents edit files directly
- avoid running future milestones without user approval

### Default subagent policy

Default behavior:

```txt
drafting = single parent agent
light review = optional read-only subagents
integration review = read-only subagents allowed
final review = read-only subagents recommended
edits = parent agent only
```

### Milestone control

Do not run the entire plan in one pass.

Complete work in bounded batches:

```txt
one milestone at a time
```

or:

```txt
one explicitly requested milestone range at a time
```

After each milestone or requested batch, report:

- files created or changed
- file states updated
- review findings
- accepted edits
- verification performed
- recommended next milestone

## Review policy

Review is a loop, but review depth depends on the stage.

### Light review

Run light review after each file or small dependency group.

Light review checks only:

- scope drift
- obvious duplication
- contradictions
- missing cross-links
- examples that conflict with existing standards
- places where a link should replace repeated explanation

Light review should not request:

- full rewrites
- prose polish
- exhaustive consistency passes
- new sections outside the file contract
- README-level organization changes

### Final review

Run final review after all standards and templates exist.

Final review checks:

- duplication across files
- contradictions
- inconsistent terminology
- unclear file boundaries
- missing discovery links
- standard-to-template links
- template-to-standard links
- examples that do not follow the handbook standards
- templates that do not reflect the standards
- missing index links

Use light review frequently and final review once.

## Linking policy

Links should improve discovery and reduce duplication.

### Link when

- another standard owns the detailed rule
- a template implements a standard
- an index page should help readers find related files
- two files have a dependency relationship
- repeated explanation can be replaced with a reference

### Do not link when

- the target file is out of scope
- the target file does not exist and is not planned
- the link would be noisy
- the local explanation is short and necessary for context

### Reviewer linking responsibilities

The reviewer should identify:

- duplicate rules that should become links
- missing related-standard links
- missing standard-to-template links
- missing template-to-standard links
- missing index-page links
- link opportunities that make discovery easier

## Subagent policy

Use subagents to reduce context pollution and avoid context rot.

The main thread should stay focused on:

- requirements
- decisions
- file contracts
- accepted changes
- final outputs

Subagents should handle bounded work and return distilled findings instead of raw logs, long exploration notes, or noisy intermediate output.

### When to use subagents

Use subagents for:

- read-heavy exploration
- reference review
- comparing a draft against a file contract
- light review of one file
- final review split by concern
- link discovery
- duplication checks
- checking examples against already-written standards

### When not to use subagents

Do not use subagents for:

- writing the same file in parallel
- multiple agents editing overlapping files
- recursive delegation
- small edits that one agent can do directly
- broad rewrites without a file contract
- tasks where coordination cost is higher than the work

### Main-thread rule

The main thread owns the source of truth.

Subagents may recommend changes, but the main thread decides:

- which findings are accepted
- which file contracts change
- which stable files reopen
- which edits are applied
- when a file moves to the next state

### Subagent output format

Subagents should return concise structured summaries.

Preferred output:

```txt
1. Findings
2. Evidence
3. Recommended targeted edits
4. Open questions
5. Files affected
```

Avoid returning:

- raw command logs
- long scratchpad notes
- unrelated observations
- full-file rewrites unless explicitly requested

### Recommended subagent roles

#### File reviewer

Use for light review of one drafted file.

Focus:

- scope drift
- duplication
- contradictions
- missing links
- examples that conflict with dependencies

#### Link mapper

Use after several files exist.

Focus:

- cross-links between related standards
- index-page discovery links
- standard-to-template links
- template-to-standard links
- places where links can replace repeated explanations

#### Reference distiller

Use when a file has reference links.

Focus:

- extract only the relevant rules
- avoid long summaries
- identify what should become local handbook guidance
- identify what should remain a reference link

#### Final reviewer

Use near the end.

Focus:

- handbook-wide consistency
- duplicated rules
- unclear file boundaries
- missing discovery links
- templates reflecting standards

### Parallel subagent limits

Prefer shallow parallelism.

Use:

```txt
max depth = 1
```

For this handbook, parallel subagents should usually be read-only reviewers, not writers.

### Suggested custom agents

If project-scoped custom agents are useful, define them under:

```txt
.codex/agents/
```

Suggested agents:

```txt
handbook_reviewer
link_mapper
reference_distiller
template_reviewer
```

These agents should default to read-only behavior unless explicitly assigned an editing task.

Do not create these agent files until the workflow actually needs them.

## Agent workflow

Use a hybrid workflow:

```txt
sequential foundation
parallel independent drafts when safe
sequential integration
```

## Agent roles

### Planner agent

Owns:

```txt
PLAN.md
```

Responsibilities:

- maintain this plan
- update task order when needed
- track dependencies
- prevent scope creep
- keep project-template details out of this plan

The planner should not rewrite standards unless explicitly assigned.

### Writer agents

Each writer agent owns exactly one assigned file at a time.

Responsibilities:

- write the assigned file
- follow the file contract
- stay within the file scope
- avoid duplicating rules from adjacent files
- use concise Markdown
- include practical examples where useful
- add cross-links when a related rule belongs in another file instead of duplicating it

Writer agents should not edit unrelated files.

### Reviewer agent

Owns consistency and discovery review.

Responsibilities:

- find duplicate rules
- find contradictions
- check terminology
- check dependency boundaries
- suggest file moves only when necessary
- identify missing examples
- identify cross-link opportunities
- identify places where a link should replace duplicated explanation
- identify index-page links that improve discovery
- identify related-template links from standards to templates
- identify related-standard links from templates back to standards

The reviewer should produce findings before large rewrites.

### Integrator agent

Owns final cleanup.

Responsibilities:

- apply accepted reviewer edits
- align headings and terminology
- add useful cross-links
- remove duplicated explanations after links are added
- preserve each file's scope
- write README files last
- ensure the handbook feels cohesive

## Input policy

Some standards may start from existing drafts and reference links.

### Existing drafts

When an existing draft is provided:

- use it as the starting point
- preserve existing decisions unless they conflict with the file contract
- do not replace the draft with a generic rewrite
- identify issues before rewriting major sections
- keep useful language when it already fits

### Reference links

When reference links are provided:

- use them to clarify the standard
- do not turn the standard into a tutorial
- do not summarize references at length
- cite or link references only when the standard benefits from direct attribution
- prefer turning reference material into concise local rules

### Missing dependencies

When a file depends on another file that does not exist yet:

- do not guess the missing standard
- add a TODO
- continue with sections that are safe to write

## Tooling boundary

The handbook should avoid mixing language standards with runtime or toolchain-specific rules.

### Runtime-agnostic standards

These files should avoid Node-specific or Deno-specific assumptions unless an example is clearly labeled:

```txt
standards/code/documentation.md
standards/code/javascript.md
standards/code/typescript.md
standards/code/jsdoc.md
```

### Tool-specific standard

Toolchain decisions belong in:

```txt
standards/tooling.md
```

That file should define:

- package manager preference
- linting preference
- formatting preference
- Markdown formatting preference
- when to use Node-oriented tooling
- when to use Deno-oriented tooling
- when to use Biome
- when to use Prettier
- when to use ESLint
- how to avoid mixing toolchains without a reason

### Node vs Deno model

Use this separation:

```txt
JavaScript standard = runtime-agnostic language patterns
TypeScript standard = runtime-agnostic type patterns
Tooling standard = environment and toolchain choices
Future runtime-specific files = only if needed
```

Do not add runtime-specific standards yet.

If the handbook later needs them, add them only when real repeated content does not fit the agnostic standards or tooling standard.

### Formatting the handbook itself

For this handbook repo:

- Use Prettier for Markdown formatting.
- Do not add Deno solely to format Markdown.
- Do not require Biome for Markdown formatting.
- Add Biome only if the repo gains JavaScript or TypeScript files that need linting or formatting.
- Use ESLint only when a project needs rules Biome does not cover.
- Use Deno tooling only for Deno-specific projects.

Suggested scripts if package tooling is added:

```json
{
  "scripts": {
    "format": "prettier --write .",
    "format:check": "prettier --check ."
  }
}
```

If JavaScript or TypeScript scripts are added later, add JS/TS checks separately instead of forcing all formatting through one tool.

## File dependency map

```txt
PLAN.md
└─ coordinates the handbook build

AGENTS.md
└─ guides all AI work

standards/tooling.md
├─ informs examples and scripts
└─ defines toolchain boundaries

standards/workflow/commits.md
├─ uses tooling vocabulary when needed
└─ guides future repo history

standards/code/documentation.md
├─ defines general documentation principles
├─ informs JavaScript and TypeScript examples
└─ informs JSDoc usage

standards/code/javascript.md
└─ standards/code/typescript.md

standards/code/documentation.md
standards/code/javascript.md
standards/code/typescript.md
└─ standards/code/jsdoc.md

standards/workflow/commits.md
└─ standards/workflow/pull-requests.md
   └─ standards/workflow/versioning.md
      └─ standards/workflow/releases.md

standards/*
└─ templates/*

standards/*
templates/*
└─ README files
```

## Work order

### Final ordered sequence

```txt
0. PLAN.md
1. AGENTS.md
2. standards/tooling.md
3. standards/workflow/commits.md
4. standards/code/documentation.md
5. standards/code/javascript.md
6. standards/code/typescript.md
7. standards/code/jsdoc.md
8. standards/workflow/pull-requests.md
9. standards/workflow/versioning.md
10. standards/workflow/releases.md
11. integration and linking review
12. templates
13. final review
14. README files
```

## Parallelization policy

### Sequential foundation

These files should be written one at a time:

```txt
PLAN.md
AGENTS.md
standards/tooling.md
standards/workflow/commits.md
standards/code/documentation.md
standards/code/javascript.md
standards/code/typescript.md
standards/code/jsdoc.md
```

Reason:

- they establish shared rules
- they depend on earlier files
- they create high risk of duplication or drift
- JSDoc examples should follow documentation, JavaScript, and TypeScript standards

### Limited parallel drafting

After the foundation exists, these may be drafted in parallel:

```txt
standards/workflow/pull-requests.md
standards/workflow/versioning.md
```

Conditions:

- one agent per file
- no agent edits another file
- each agent reads relevant dependency files first
- light review happens after drafting

### Sequential release standard

Write this after `versioning.md`:

```txt
standards/workflow/releases.md
```

Reason:

- release rules depend on versioning rules
- release rules should link back to commit and versioning standards

### Parallel templates after standards

After standards are stable and reviewed, templates may be created in parallel:

```txt
templates/agents/AGENTS.md
templates/github/pull_request_template.md
templates/releases/changeset.md
templates/releases/release-notes.md
```

Conditions:

- templates reflect standards
- templates do not introduce new standards
- templates stay generic enough to copy into multiple projects
- templates link back to relevant standards when useful

### README files are always last

Write these after standards and templates are stable:

```txt
standards/README.md
templates/README.md
README.md
```

## Milestones

## Milestone 0: Plan and scaffold

Create the initial folder structure and this plan.

Files:

```txt
README.md
AGENTS.md
PLAN.md
standards/.gitkeep
standards/code/.gitkeep
standards/workflow/.gitkeep
templates/agents/.gitkeep
templates/github/.gitkeep
templates/releases/.gitkeep
```

Keep `README.md` minimal.

Suggested commit:

```txt
chore(meta): 🎉 scaffold handbook repo
```

## Milestone 1: Agent instructions

Write:

```txt
AGENTS.md
```

Purpose:

Define how AI agents should edit the handbook.

Contract:

```txt
Owns:
- agent editing behavior
- scope control
- anti-rewrite rules
- linking preference
- file ownership rules
- review expectations

Does not own:
- tooling choices
- commit format
- JavaScript style
- TypeScript style
- JSDoc tags
- release process
```

Important rule:

```txt
AGENTS.md should be tooling-aware but not tooling-specific.
```

It may say:

```txt
Follow standards/tooling.md when tooling guidance exists.
```

It should not define tooling choices itself.

Suggested commit:

```txt
docs(agents): 📝 add handbook agent instructions
```

## Milestone 2: Tooling standard

Write:

```txt
standards/tooling.md
```

Purpose:

Define preferred tools, formatting choices, and runtime/toolchain boundaries.

Contract:

```txt
Owns:
- package manager preferences
- formatter preferences
- linter preferences
- Markdown formatting
- Node vs Deno boundary
- Biome vs Prettier vs ESLint boundary
- dependency policy
- when project-specific tooling overrides defaults

Does not own:
- JavaScript code style
- TypeScript type rules
- JSDoc tags
- commit format
- release process
```

Suggested commit:

```txt
docs(tooling): 📝 add tooling standard
```

## Milestone 3: Commit standard

Write:

```txt
standards/workflow/commits.md
```

Purpose:

Define commit conventions before the repo accumulates much history.

Use the existing commit guide as the source draft.

Contract:

```txt
Owns:
- commit title format
- allowed tags
- scope rules
- gitmoji mapping
- commit body rules
- breaking change commit rules
- release commit constraints
- commit review checklist

Does not own:
- full release algorithm
- full versioning rules
- pull request structure
- tooling setup
```

Known source draft decisions:

- commit titles use a structured tag, scope, gitmoji, and title format
- commit bodies are optional
- commit bodies use bullets only
- tags map to version pressure
- gitmoji must match the selected tag
- scopes describe the affected area
- `unstable` marks non-stable surface area
- breaking changes must be visible in the title
- release commits are reserved for automation
- the review checklist validates format, intent, scope, body, breaking changes, and release intent

Potential review questions:

- Should scope be required or optional for all tags?
- Should docs-only changes always create patch pressure?
- Should release commit rules stay here or mostly link to `releases.md`?
- Should tooling validation for commits be defined here or linked from `tooling.md`?

Suggested commit:

```txt
docs(workflow): 📝 add commit standard
```

## Milestone 4: Code documentation standard

Write:

```txt
standards/code/documentation.md
```

Purpose:

Define general code documentation philosophy before language-specific documentation.

Contract:

```txt
Owns:
- when to document code
- when not to comment
- documenting public APIs
- documenting constraints
- documenting tradeoffs
- documenting examples
- avoiding comments that repeat code
- linking instead of duplicating explanations

Does not own:
- JSDoc tag syntax
- JavaScript style
- TypeScript type-safety rules
- tooling choices
- README writing
```

Suggested commit:

```txt
docs(code): 📝 add code documentation standard
```

## Milestone 5: JavaScript standard

Write:

```txt
standards/code/javascript.md
```

Purpose:

Define runtime-agnostic JavaScript code style for both JavaScript and TypeScript.

Contract:

```txt
Owns:
- naming
- imports
- exports
- modules
- functions
- async code
- error handling
- null and undefined
- object and array patterns
- iteration
- comments at a general level
- file organization
- runtime-agnostic examples

Does not own:
- TypeScript-only type rules
- JSDoc mechanics
- formatter/linter choices
- Node-specific APIs
- Deno-specific APIs
- React-specific conventions
```

Suggested commit:

```txt
docs(code): 📝 add JavaScript standard
```

## Milestone 6: TypeScript standard

Write:

```txt
standards/code/typescript.md
```

Purpose:

Define TypeScript-specific additions that extend `javascript.md`.

Contract:

```txt
Owns:
- strict TypeScript
- avoiding any
- avoiding unsafe casts
- fixing types at the source
- unknown at boundaries
- runtime validation at boundaries
- satisfies
- type imports and exports
- generics
- discriminated unions
- public API typing

Does not own:
- general JavaScript style
- formatter/linter choices
- React-specific conventions
- JSDoc mechanics
- long TypeScript tutorials
```

Suggested commit:

```txt
docs(code): 📝 add TypeScript standard
```

## Milestone 7: JSDoc standard

Write:

```txt
standards/code/jsdoc.md
```

Purpose:

Define exact JSDoc usage for JavaScript and TypeScript projects.

Dependencies:

```txt
standards/code/documentation.md
standards/code/javascript.md
standards/code/typescript.md
```

Contract:

```txt
Owns:
- approved JSDoc tags
- discouraged JSDoc tags
- JSDoc examples
- JSDoc formatting conventions
- JSDoc usage for public APIs
- JSDoc usage for deprecations
- JSDoc usage for internal APIs
- JSDoc links

Does not own:
- general documentation philosophy
- general JavaScript style
- TypeScript type-safety rules
- formatter/linter choices
```

Important rule:

```txt
JSDoc examples must follow the JavaScript and TypeScript standards.
```

Suggested commit:

```txt
docs(code): 📝 add JSDoc standard
```

## Milestone 8: Pull request standard

Write:

```txt
standards/workflow/pull-requests.md
```

Purpose:

Define pull request expectations.

Contract:

```txt
Owns:
- PR size
- PR title expectations
- PR body expectations
- screenshots or recordings
- testing notes
- linked issues
- reviewability
- breaking change notes
- when not to open a PR yet

Does not own:
- commit format
- versioning algorithm
- release process
```

Should link to:

```txt
standards/workflow/commits.md
standards/workflow/versioning.md
templates/github/pull_request_template.md
```

Suggested commit:

```txt
docs(workflow): 📝 add pull request standard
```

## Milestone 9: Versioning standard

Write:

```txt
standards/workflow/versioning.md
```

Purpose:

Define version meaning and change classification.

Contract:

```txt
Owns:
- semantic versioning meaning
- patch changes
- minor changes
- major changes
- prerelease versions
- breaking changes
- unstable surface area
- versioning examples
- relationship to commit tags

Does not own:
- full release checklist
- release notes template
- publishing process
```

Should link to:

```txt
standards/workflow/commits.md
standards/workflow/releases.md
templates/releases/changeset.md
```

Suggested commit:

```txt
docs(workflow): 📝 add versioning standard
```

## Milestone 10: Release standard

Write:

```txt
standards/workflow/releases.md
```

Purpose:

Define the release process using the versioning standard.

Contract:

```txt
Owns:
- release checklist
- changelog expectations
- release notes expectations
- release tags
- publishing expectations
- release artifacts
- prerelease flow
- post-release verification

Does not own:
- commit title format
- version meaning
- PR structure
```

Should link to:

```txt
standards/workflow/commits.md
standards/workflow/versioning.md
templates/releases/release-notes.md
```

Suggested commit:

```txt
docs(workflow): 📝 add release standard
```

## Milestone 11: Integration and linking review

Run a review pass across all standards before creating templates.

Review goals:

- remove duplicate rules
- resolve contradictions
- align terminology
- ensure each file has a clear scope
- ensure general documentation precedes JSDoc
- ensure JSDoc examples follow JS/TS standards
- ensure TypeScript extends JavaScript
- ensure releases depend on versioning
- make headings consistent
- make examples consistent
- identify cross-link opportunities
- replace duplicated explanations with links where appropriate
- identify index-page links for discovery
- identify links from standards to related templates
- identify links from templates to related standards

Suggested reviewer prompt:

```txt
Review these handbook standards for consistency and discovery.

Goals:
- remove duplicate rules
- find contradictions
- align terminology
- confirm each file has a clear scope
- ensure general documentation precedes JSDoc
- ensure JSDoc examples follow the JavaScript and TypeScript standards
- ensure TypeScript extends JavaScript
- ensure releases depend on versioning
- identify cross-link opportunities
- identify where links should replace repeated explanation
- identify index-page links that improve discovery
- identify standard-to-template and template-to-standard links
- keep the standards concise and practical

Return:
1. Issues found
2. Suggested file moves
3. Duplicate rules to remove
4. Link opportunities
5. Missing examples
6. Specific edits
```

Suggested commit after accepted edits:

```txt
docs(*): 📝 align handbook standards
```

## Milestone 12: File templates

Create file-level templates after standards are reviewed.

Files:

```txt
templates/agents/AGENTS.md
templates/github/pull_request_template.md
templates/releases/changeset.md
templates/releases/release-notes.md
```

### `templates/agents/AGENTS.md`

Purpose:

Copyable agent instructions for individual projects.

Should include:

- project overview placeholder
- commands placeholder
- workflow rules
- type-safety expectations
- testing/check expectations
- final response expectations
- links to relevant standards where useful

Suggested commit:

```txt
docs(templates): 📝 add agent instruction template
```

### `templates/github/pull_request_template.md`

Purpose:

Copyable GitHub PR template.

Should include:

- summary
- changes
- screenshots or recordings
- testing
- breaking changes
- checklist
- links to PR standard where useful

Suggested commit:

```txt
docs(templates): 📝 add pull request template
```

### `templates/releases/changeset.md`

Purpose:

Copyable Changesets entry template.

Should include:

- package name
- version bump type
- summary
- user-facing impact
- migration notes if needed
- links to versioning standard where useful

Suggested commit:

```txt
docs(templates): 📝 add changeset template
```

### `templates/releases/release-notes.md`

Purpose:

Copyable release notes template.

Should include:

- highlights
- changes
- fixes
- breaking changes
- migration notes
- verification
- links to release standard where useful

Suggested commit:

```txt
docs(templates): 📝 add release notes template
```

## Milestone 13: Final review

Run the final deep review after standards and templates exist.

Final review goals:

- verify all standards have clear ownership
- verify templates reflect standards
- verify index pages can be written without discovering missing structure
- verify linking improves discovery
- verify duplicated explanations have been reduced
- verify project templates remain out of scope
- verify README files can summarize the finished repo

Suggested reviewer prompt:

```txt
Run the final review for the handbook.

Review all standards and templates.

Goals:
- verify file boundaries
- verify templates reflect standards
- find remaining duplication
- find contradictions
- find missing discovery links
- find missing template-to-standard links
- check that project templates remain out of scope
- identify edits needed before writing README files

Return:
1. Must-fix issues
2. Should-fix issues
3. Link opportunities
4. README/index implications
5. Final integration edits
```

Suggested commit after accepted edits:

```txt
docs(*): 📝 finalize handbook standards and templates
```

## Milestone 14: README files

Write README files last.

Order:

```txt
standards/README.md
templates/README.md
README.md
```

### `standards/README.md`

Purpose:

Index the standards and explain what belongs in standards.

Should include:

- standards purpose
- tooling standard
- code standards list
- workflow standards list
- dependency order
- how to use standards
- what not to put in standards
- links to all standards

Suggested commit:

```txt
docs(standards): 📝 add standards index
```

### `templates/README.md`

Purpose:

Index the templates and explain how to use them.

Should include:

- templates purpose
- available templates
- when to copy templates
- how templates relate to standards
- boundary between file templates and project templates
- links to all templates
- links to related standards

Suggested commit:

```txt
docs(templates): 📝 add templates index
```

### Root `README.md`

Purpose:

Summarize the finished handbook.

Should include:

- what this repo is
- what it contains
- what it does not contain
- repo structure
- usage guidance
- relationship between standards and templates
- links to standards and templates indexes

Suggested commit:

```txt
docs(*): 📝 complete handbook README
```

## Recommended task queue

### Sequential foundation

- [ ] Scaffold repo
- [ ] Write `AGENTS.md`
- [ ] Light review `AGENTS.md`
- [ ] Write `standards/tooling.md`
- [ ] Light review `standards/tooling.md`
- [ ] Write `standards/workflow/commits.md`
- [ ] Light review `standards/workflow/commits.md`
- [ ] Write `standards/code/documentation.md`
- [ ] Light review `standards/code/documentation.md`
- [ ] Write `standards/code/javascript.md`
- [ ] Light review `standards/code/javascript.md`
- [ ] Write `standards/code/typescript.md`
- [ ] Light review `standards/code/typescript.md`
- [ ] Write `standards/code/jsdoc.md`
- [ ] Light review `standards/code/jsdoc.md`

### Workflow standards

- [ ] Write `standards/workflow/pull-requests.md`
- [ ] Light review `standards/workflow/pull-requests.md`
- [ ] Write `standards/workflow/versioning.md`
- [ ] Light review `standards/workflow/versioning.md`
- [ ] Write `standards/workflow/releases.md`
- [ ] Light review `standards/workflow/releases.md`

### Integration

- [ ] Run integration and linking review across standards
- [ ] Apply accepted integration edits
- [ ] Mark standards stable

### Templates

- [ ] Write `templates/agents/AGENTS.md`
- [ ] Write `templates/github/pull_request_template.md`
- [ ] Write `templates/releases/changeset.md`
- [ ] Write `templates/releases/release-notes.md`
- [ ] Light review templates

### Finalization

- [ ] Run final deep review
- [ ] Apply accepted final edits
- [ ] Write `standards/README.md`
- [ ] Write `templates/README.md`
- [ ] Write root `README.md`
- [ ] Mark initial handbook frozen

## Prompt pattern for file contracts

Use this before drafting a file:

```txt
Create a file contract for this handbook file.

File:
[FILE PATH]

Known dependencies:
[FILES]

Known source inputs:
[DRAFTS OR LINKS]

Return:
1. Purpose
2. Owns
3. Does not own
4. Dependencies
5. Link expectations
6. Open questions
```

## Prompt pattern for writer agents

Use this prompt for one file at a time:

```txt
You are helping write my personal engineering handbook.

File to write:
[FILE PATH]

File contract:
[CONTRACT]

Source inputs:
[EXISTING DRAFTS OR LINKS]

Relevant dependencies:
[FILES THIS ONE SHOULD EXTEND OR AVOID DUPLICATING]

Linking expectations:
- Add links to related standards when useful.
- Prefer links over repeated explanations.
- Do not create links to files that are out of scope or not planned.

Preferences:
- Keep it concise and practical.
- Prefer rules with small examples.
- Do not duplicate rules from adjacent files.
- Do not invent unrelated conventions.
- Preserve the standards/templates split.
- Use Markdown.

Write the first complete draft.
```

## Prompt pattern for light review

Use this after a file or small dependency group is drafted:

```txt
Light review this handbook file.

File:
[FILE]

Known dependencies:
[FILES]

Review only for:
- scope drift
- obvious duplication
- contradictions
- missing cross-links
- examples that conflict with existing standards

Do not rewrite the file.
Do not do prose polish.
Return only high-impact issues and targeted edits.
```

## Prompt pattern for subagent review

Use this when the parent agent is allowed to spawn review subagents:

```txt
Spawn read-only subagents for this review step.

Use one subagent per review concern:
1. Scope drift
2. Duplication and contradictions
3. Cross-link opportunities
4. Example alignment

Wait for all subagents.

Each subagent should return:
1. Findings
2. Evidence
3. Recommended targeted edits
4. Open questions
5. Files affected

Do not let subagents edit files.
Do not return raw logs.
The parent agent should consolidate findings before applying any edits.
```

## Prompt pattern for final review

Use this after standards and templates exist:

```txt
Run the final review for the handbook.

Files:
[LIST FILES]

Goals:
- find remaining duplication
- find contradictions
- identify unclear file boundaries
- check whether each file stays in scope
- identify where links should replace duplicated explanation
- identify cross-links that improve discovery
- identify missing links from index pages
- identify related template links
- verify templates reflect standards
- verify README files can be written from the final structure

Return:
1. Must-fix issues
2. Should-fix issues
3. Suggested edits by file
4. Rules to move
5. Rules to delete
6. Link opportunities
7. README/index implications
```

## Prompt pattern for integrator agents

Use this after review:

```txt
You are integrating reviewed edits into my handbook.

Files to update:
[LIST FILES]

Accepted review findings:
[FINDINGS]

Goals:
- apply accepted reviewer feedback
- align terminology
- add useful cross-links
- remove duplicated explanations after adding links
- preserve each file's scope
- keep Markdown concise
- avoid introducing new standards unless necessary

Make focused edits only.
```

## Kickoff prompt pattern

Use this when handing the plan to Codex:

```txt
Read PLAN.md and follow it as the source of truth.

You are the parent/orchestrator agent.

Execution rules:
- Do not complete the whole plan in one run.
- Complete only the requested milestone or milestone range.
- Before editing, summarize the planned file changes.
- After editing, summarize changed files, file states, review findings, and verification.
- Keep the main thread focused on decisions, accepted edits, and final outputs.
- Do not rewrite stable files unless their contract changes.

Subagent rules:
- You may spawn subagents only at review steps that explicitly allow subagents.
- Subagents must be read-only unless I explicitly grant write permission.
- Do not spawn subagents during drafting.
- Do not let subagents edit files.
- Wait for all subagents before applying review edits.
- Summarize subagent findings before changing files.
- Apply only accepted, targeted edits.

Start with:
[REQUESTED MILESTONE OR RANGE]
```

## Definition of done

The initial handbook is done when:

- all core standards exist
- all file-level templates exist
- README files are written last
- duplicated rules have been removed or replaced with links
- useful cross-links have been added
- file boundaries are clear
- examples follow the appropriate standards
- project templates are explicitly out of scope
- agents can use the handbook without needing extra explanation
- the initial set of files is marked frozen

## Long-term follow-ups

After the initial handbook is stable, consider adding more sections only when there is a clear need.

Possible future categories:

```txt
guides/
cookbook/
configs/
scripts/
```

Possible future planning topics:

```txt
project templates
config packages
scaffolding CLI
documentation site
```

These are not part of the initial plan.
