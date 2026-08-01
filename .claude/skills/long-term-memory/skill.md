# Long Term Memory

These instructions are for managing long-term memory in this project.
If you have specific instructions for issue/task tracking, then only use these instructions for non-issue-related memory.

## Why Long-Term Memory?

You work across multiple sessions. Sometimes you need to pause and return later, or switch contexts. Sometimes the
work gets interrupted by crashes, other tasks, or simply the need to reflect. Long-term memory helps you maintain continuity across these breaks.
Think of it as a persistent notebook that captures your evolving understanding, decisions, and open questions of both the project and the team.

### What to Record
- Insights learned about the project, its architecture, and its dependencies.
- Your personal understanding of the codebase, the team and its patterns.
- Deferred questions or assumptions that need validation.

### What NOT to Record

- Do NOT duplicate information already captured in issue trackers, documentation, or code comments.
- Details that should be ADRs (Architecture Decision Records) or formal documentation. Use this memory for your personal understanding and open threads only.

## Working Memory Workflow

- Maintain session-to-session context in `memory.md` (kept at the repo root).
- Use the format: `### [YYYY-MM-DD HH:MM] - [memory]` for each entry.
- You MUST use the operating system's date and time for timestamps, do not fake or invent timestamps.
- At the start of each session, skim the most recent entries to refresh open threads and outstanding follow-ups.
- As new facts, decisions, edge cases, or follow-ups emerge, jot them down immediately—do not wait for the session to end.
- When you pause or wrap up, ensure the latest entry captures what changed, assumptions requiring validation, and concrete next steps or experiments to try.
- If a note is resolved or obsolete, annotate it inline rather than deleting—future Bob needs the breadcrumb trail.
- Periodically tidy the notebook by folding fully resolved items into a short “Resolved” summary so open questions remain easy to scan.
- Keep the tone factual and concise; this notebook is for internal continuity, not user-facing documentation.
- You may create separate sections for different topics if that helps organization, but keep everything in a single `memory.md` file.
- Avoid duplicating information already captured in issue trackers or documentation—focus on your personal understanding and open threads.
- Review and update this memory file at the start and end of each session to ensure it reflects your current state of knowledge and outstanding questions.
- When wrapping up a task, scan the memory for any entries that can be expunged or consolidated to keep it relevant and focused.
