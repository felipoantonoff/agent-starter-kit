---
shortDescription: Long-term memory across sessions.
usedBy: [maestro]
version: 0.1.0
lastUpdated: 2026-03-10
---

## Purpose

Agents start cold every session — lessons learned and user preferences vanish the moment a conversation ends. This skill defines a file-based long-term memory that accumulates project knowledge across sessions. Feedback given once is remembered forever.

## Procedure

1. **Check for the memory directory.** Look for `.memory/` at the project root. If it does not exist, create it with `long-term.md` initialized with the section headers from the schema below.

2. **Read long-term memory at session start.** Read `.memory/long-term.md`. This step is read-only — do not modify long-term memory here.

3. **Write to long-term memory on feedback.** When the user provides feedback or corrections on a sub-agent's output, extract the lesson and append it to the appropriate section of `long-term.md`.

   - One line per entry.
   - Before appending, scan the section for duplicates or contradictions. If a new entry contradicts an existing one, replace the old entry.

4. **Distill before closing.** When the session ends, review the conversation and extract any reusable lessons — patterns that worked, mistakes to avoid, user preferences revealed. Append them to the appropriate sections of `long-term.md` (same deduplication rules as step 3).

## Schema

```markdown
## Preferences

- <one preference per line>

## Feedback

- <one feedback entry per line>

## Learned Rules

- <one rule per line>

## Discovered Issues

- <one issue per line — pre-existing bugs, tech debt, or code smells found during work but outside the current task's scope>

## Project Notes

- <one note per line>
```

The five sections above are the defaults. Maestro may create additional sections when an entry does not fit any existing one. New sections follow the same format.

**Schema notes:**

- Sections are append-only by agents. The user may prune or reorganize manually.
- Entries are plain text, one line each. No nested lists, no multi-line blocks.

## Guardrails

- Never write speculatively to long-term memory. Only record what the user explicitly stated or what a sub-agent's output concretely demonstrated.
- Never store sensitive data (credentials, tokens, secrets) in memory files.
- Never let sub-agents write directly to `.memory/`. Memory writes are Maestro's responsibility — sub-agents return output, Maestro decides what to remember.
