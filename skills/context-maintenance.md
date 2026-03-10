---
shortDescription: How to maintain .context.md files as the project evolves.
usedBy: [coder, contextualizer]
version: 0.1.0
lastUpdated: 2026-03-10
---

## Purpose

Every directory in a project has a `.context.md` that orients whoever arrives next — human or agent. These files answer "what lives here" so that agents and developers can navigate the codebase without reading every file. This skill defines when and how to keep them accurate as the project evolves, and the schema they must follow.

## Procedure

1. **Scope.** Prefer scanning from `src/` as the root. Skip hidden/dot directories (`.vscode`, `.claude`, `.git`, `.github`, etc.) and generated, vendored, or ephemeral directories (`node_modules`, `dist`, `.cache`, `__pycache__`, `vendor`).

2. **Determine if an update is needed.** A `.context.md` update is required when a change alters the purpose of a directory, adds or removes files/dependencies, or changes naming conventions. Bug fixes, formatting, and refactors that preserve structure do not require updates.

3. **Write or update the `.context.md` inside the affected directory following the schema below.**

4. **Commit together.** The `.context.md` update MUST be in the same commit as the structural change. Do not defer.

## Schema

```markdown
<context path="relative/path" updated="YYYY-MM-DD">

One to two sentences describing what this directory contains and why it
exists.

## Summary

- filename.ext — short description of what this file does
- filename.ext — short description of what this file does
- subdirectory/ — short description of what this subdirectory contains

## Constraints

- MUST / MUST NOT statements. Non-negotiable constraints specific to this directory.

## Guidance

- SHOULD / SHOULD NOT statements. Recommendations that may be deviated from with justification.

</context>
```

**Schema notes:**

- Constraints and Guidance are optional — only include them when the directory has rules worth stating.
- Keep it short. This file will be read frequently by multiple agents.

## Guardrails

- Never invent purpose. If a directory's role is unclear after reading its contents, say so.
- If a constraint looks like it should apply project-wide rather than to this directory alone, flag it to the user but do not modify the `.agents/` directory. That directory is managed via git.
- Never update the `updated` date in a `.context.md` `<context>` tag unless the content of that file actually changed. Touching the date without a content change creates false drift signals.
