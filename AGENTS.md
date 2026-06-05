# Agent Instructions

This repo is the source of truth for reusable engineering standards and copyable file templates. Follow [PLAN.md](PLAN.md) as the controlling plan for scope, order, file states, and milestone boundaries.

## Scope Control

- Work only on the milestone or file range explicitly assigned.
- Do not create project-template content in this repo.
- Do not write standards or templates before their milestone is active.
- Use `references/` only as supporting input; do not copy reference material wholesale.
- When a dependency is missing, add a TODO only if the active file needs it to stay accurate.

## File Ownership

- Each edited file must stay within its contract in `PLAN.md`.
- Do not move a rule into a file that does not own it.
- Keep README files as discovery surfaces, not as standards.
- Keep templates as copyable files, not as new sources of policy.
- Leave stable files closed unless a dependency changes, review finds an issue, the contract changes, or the owner explicitly asks for a revision.

## Editing Workflow

Use the handbook workflow from `PLAN.md`:

```txt
contract -> draft -> light review -> targeted edits -> stable -> final review -> frozen
```

- Confirm the file contract before drafting.
- Prefer targeted edits over full rewrites.
- Do not rewrite an entire file unless its contract changes.
- Use concise Markdown with practical examples only when examples are in scope.
- Let final polish happen during integration or final review, not during every draft.

## Linking

- Prefer links over repeating rules owned by another file.
- Link when it improves discovery, clarifies a dependency, or replaces duplicated explanation.
- Do not add noisy links or links to out-of-scope files.
- If tooling guidance is needed and `standards/tooling.md` exists, follow it instead of defining tooling choices here.

## Review Expectations

- Run light review after each file or small dependency group.
- Light review checks scope drift, obvious duplication, contradictions, missing cross-links, and examples that conflict with existing standards.
- Do not use light review for broad rewrites, exhaustive prose polish, or new sections outside the file contract.
- Run final review only after all standards and templates exist.

## Subagents

- Use subagents only at review steps where the active instructions allow them.
- Subagents must be read-only unless the owner explicitly grants write permission.
- Do not spawn subagents during drafting.
- Wait for all subagents before applying review edits.
- Summarize subagent findings, accept only targeted findings, then apply edits as the parent agent.
