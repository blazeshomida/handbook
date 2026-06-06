# Agent Instructions

This repo is the source of truth for reusable engineering standards and copyable
file templates.

## Scope Control

- Work only on the file or topic range explicitly assigned.
- Do not create project-template content in this repo.
- Use local references only as supporting input; do not copy reference material
  wholesale.
- When a dependency is missing, add a TODO only if the active file needs it to
  stay accurate.

## File Ownership

- Do not move a rule into a file that does not own it.
- Keep README files as discovery surfaces, not as standards.
- Keep templates as copyable files, not as new sources of policy.
- Write standards and templates for people and agents who want to know how to
  write the thing named by the file.
- Leave existing files closed unless a dependency changes, review finds an
  issue, or the owner explicitly asks for a revision.

## Editing Workflow

- Prefer targeted edits over full rewrites.
- Do not rewrite an entire file unless the requested behavior requires it.
- Use concise Markdown with practical examples only when examples are in scope.
- Before editing, summarize the planned file changes.
- After editing, summarize changed files, review findings, and verification.

## Linking

- Prefer links over repeating rules owned by another file.
- Link when it improves discovery, clarifies a dependency, or replaces duplicated explanation.
- Do not add noisy links or links to out-of-scope files.
- If tooling guidance is needed and `standards/tooling.md` exists, follow it instead of defining tooling choices here.

## Review Expectations

- Run light review after each file or small dependency group when changing
  policy or templates.
- Light review checks scope drift, obvious duplication, contradictions, missing cross-links, and examples that conflict with existing standards.
- Do not use light review for broad rewrites, exhaustive prose polish, or
  unrelated new sections.

## Subagents

- Use subagents only when the user explicitly allows them.
- Subagents must be read-only unless the owner explicitly grants write permission.
- Do not spawn subagents during drafting.
- Wait for all subagents before applying review edits.
- Summarize subagent findings, accept only targeted findings, then apply edits as the parent agent.
