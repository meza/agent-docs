# Persistent Task Memory

This project uses **Beads** for issue tracking.

## Issue Tracking Workflow with bd (beads)

**IMPORTANT**: This project uses **bd (beads)** for ALL issue tracking. Do NOT use markdown TODOs, task lists, or other tracking methods.

YOU MUST NOT DIRECTLY EDIT OR READ FILES IN THE `.beads/` FOLDER. Use the `bd` CLI tool or `mcp__beads__*` functions to manage issues.

## Parent/Child Semantics (Imperative)

Any clustering/grouping MUST preserve causality: a parent issue MUST NOT be considered ready or done while any of its children are still open.

In Beads, dependency direction matters:
- `bd --no-db dep add <issue-id> <depends-on-id> --type blocks` means `<depends-on-id>` blocks `<issue-id>`.
- For parent/child work, the parent MUST be blocked by each child (not the other way around).

Required pattern for child tickets:
1. Create the child under the parent: `bd --no-db create "Child title" --parent <parent-id>`
2. Make the parent blocked by the child: `bd --no-db dep add <parent-id> <child-id> --type blocks`

Optional: provenance-only discovery links may use `discovered-from:<id>`, but `discovered-from` MUST NOT be relied on to block work.

## Setting Up

If you don't have access to the `bd` CLI tool, install them first.
Only one installation method is needed and only install if you don't have it yet.

### With NPM (if you have node)

Only if you have Node.js and NPM installed. Don't install Node.js just for this!

```bash
npm install -g @beads/bd
```

### For unix systems (Linux, macOS)
```bash
curl -sSL https://raw.githubusercontent.com/steveyegge/beads/main/scripts/install.sh | bash
```

### Issue Types

- `bug` - Something broken
- `feature` - New functionality
- `task` - Work item (tests, docs, refactoring)
- `epic` - Large feature with subtasks
- `chore` - Maintenance (dependencies, tooling)

### Priorities

- `0` - Critical (security, data loss, broken builds)
- `1` - High (major features, important bugs)
- `2` - Medium (default, nice-to-have)
- `3` - Low (polish, optimization)
- `4` - Backlog (future ideas)

### Workflow

1. **Check ready work**: `bd --no-db ready` shows unblocked issues
2. **Claim your task**: `bd --no-db update <id> --status in_progress`
3. **Work on it**: Implement, test, document
4. **Discover new work under an issue?** Create a child, then block the parent:
   - Create child: `bd --no-db create "Found bug" -p 1 --parent <parent-id>`
   - Block parent on child: `bd --no-db dep add <parent-id> <child-id> --type blocks`
5. **Complete**: `bd --no-db close <id> --reason "Done"`

#### Completion Criteria

- Code changes implemented
- Tests passing
- Documentation updated
- Team/User has reviewed the changes
- Team/User explicitly asked for completion

### Auto-Sync

bd automatically syncs with git:
- Exports to `.beads/issues.jsonl` after changes (5s debounce)
- Imports from JSONL when newer (e.g., after `git pull`)
- No manual export/import needed!

### Tool functions (Recommended)

If you have access to `mcp__beads__*` functions, those are preferred over the CLI commands.

### CLI Help

Run `bd --no-db <command> --help` to see all available flags for any command.
For example: `bd --no-db create --help` shows `--parent`, `--deps`, `--assignee`, etc.

### Important Rules

- Use bd for ALL task tracking
- Always use the `--no-db --json` flags for programmatic use
- For parent/child work: ALWAYS block the parent on each child (`bd --no-db dep add <parent-id> <child-id> --type blocks`)
- `discovered-from` is provenance-only and MUST NOT be relied on as a blocker
- Check `bd --no-db ready` before asking "what should I work on?"
- Run `bd --no-db <cmd> --help` to discover available flags
- Do NOT use external issue trackers
- Do NOT duplicate tracking systems
- YOU MUST NOT DIRECTLY EDIT OR READ FILES IN THE `.beads/` FOLDER. Use the `bd` CLI tool or `mcp__beads__*` functions to manage issues.
