# Persistent Task Memory

This project uses **Beads** for issue tracking.

## Issue Tracking Workflow with bd (beads)

**IMPORTANT**: This project uses **bd (beads)** for ALL issue tracking. Do NOT use markdown TODOs, task lists, or other tracking methods.

YOU MUST NOT DIRECTLY EDIT OR READ FILES IN THE `.beads` FOLDER. Use the `bd` CLI tool to manage issues.

## Stable Interface (Text Output Only)

The `bd` CLI's default text output is the stable interface.

- Do NOT use `--json`, `--format`, or any flag that changes output format.
- Do NOT pipe Beads output to `jq`, `sed`, `awk`, or other transformation tools.
- Do NOT read or grep files under `.beads` (including `.issues.jsonl`); the on-disk format is not stable and may be mid-sync.

If you need issue data, use:
- `bd --no-db list`
- `bd --no-db ready`
- `bd --no-db show <id>`

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

### CLI Help

Run `bd --no-db <command> --help` to see all available flags for any command.
For example: `bd --no-db create --help` shows `--parent`, `--deps`, `--assignee`, etc.

### Quick Reference

#### Common Commands

```bash
bd --no-db list
bd --no-db ready
bd --no-db show <id>
bd --no-db update <id> --status in_progress
bd --no-db close <id> --reason "Done"
```

#### Dependencies

- Dependency direction is always: `<issue-id>` depends on `<depends-on-id>`.
- Default dependency type is `blocks`.
- Use `--parent` for hierarchy. Do NOT use `--type parent-child` as a substitute for blocking.

```bash
# Add a blocking dependency (depends-on blocks issue)
bd --no-db dep add <issue-id> <depends-on-id> --type blocks

# Other dependency types
bd --no-db dep add <issue-id> <other-id> --type related
bd --no-db dep add <new-id> <source-id> --type discovered-from

# Inspect dependencies
bd --no-db dep tree <issue-id>
bd --no-db dep cycles
```

#### Issue Fields

```bash
bd --no-db update <id> --description "..."
bd --no-db update <id> --notes "..."
bd --no-db update <id> --design "..."
bd --no-db update <id> --acceptance "..."
```

#### Comments

```bash
bd --no-db comments <id>
bd --no-db comments add <id> "comment text"
```

### Important Rules

- Use bd for ALL task tracking
- When running as an automated agent, always include `--no-db`
- Do NOT use `--json` output or parse/transform Beads output
- For parent/child work: ALWAYS block the parent on each child (`bd --no-db dep add <parent-id> <child-id> --type blocks`)
- `discovered-from` is provenance-only and MUST NOT be relied on as a blocker
- Check `bd --no-db ready` before asking "what should I work on?"
- Run `bd --no-db <cmd> --help` to discover available flags
- Do NOT use external issue trackers
- Do NOT duplicate tracking systems
- YOU MUST NOT DIRECTLY EDIT OR READ FILES IN THE `.beads/` FOLDER. Use the `bd` CLI tool to manage issues.
