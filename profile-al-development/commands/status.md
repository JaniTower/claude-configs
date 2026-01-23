# Command: /status

Show current workflow state, progress, and project context status using native Tasks.

## Purpose

Quick overview of:
- Current tasks and their status
- Blocked/unblocked tasks
- Project context status
- Recent agent activity

## Implementation

### Step 1: Check Tasks

Use `TaskList` to get all current tasks:

```
TaskList → Returns all tasks with:
  - id
  - subject
  - status (pending/in_progress/completed)
  - owner
  - blockedBy (list of blocking task IDs)
```

### Step 2: Check Project Context

Check if `.dev/project-context.md` exists:
- If exists: Show last updated timestamp, object count
- If not exists: Suggest `/init-context`

### Step 3: Read Session Log

Read last 5 entries from `.dev/session-log.md` to show recent activity.

### Step 4: Display Status

```
# Workflow Status

## Current Tasks

| Status | Task | Owner | Blocked By |
|--------|------|-------|------------|
| ✓ | Requirements Analysis | - | - |
| ✓ | Solution Planning | - | - |
| ⚙ | Development | al-developer | - |
| ⏳ | Code Review | - | Development |
| ⏳ | Diagnostics | - | Code Review |
| ⏳ | Testing | - | Diagnostics |
| ⏳ | Test Review | - | Testing |

**Progress:** 2/7 tasks completed
**Current:** Development (in progress)
**Next:** Code Review (waiting for Development)

## Project Context
Status: ✓ Available (.dev/project-context.md)
Last Updated: 2026-01-22 09:15:00
Objects: 12 tables, 8 pages, 15 codeunits

## Recent Activity (from session-log.md)
1. [14:25] al-developer - Implementing validation logic
2. [14:05] solution-planner - Created solution plan
3. [14:00] requirements-engineer - Analyzed requirements

## Next Steps
→ al-developer completing Development task
→ Then: Code Review will unblock
→ User approval needed after code review

---

💡 Tasks persist in ~/.claude/tasks/ - resume anytime!
```

### If No Tasks

```
# Workflow Status

No active tasks.

## Project Context
Status: ✓ Available (.dev/project-context.md)
Last Updated: 2026-01-22 09:15:00
Objects: 12 tables, 8 pages, 15 codeunits

## Available Commands
- /dev-cycle "description" - Full development cycle (creates tasks)
- /fix "bug description" - Quick bug fix (2-5 min)
- /plan "feature" - Planning only
- /develop - Development phase
- /test - Testing phase

Ready for next task!
```

### If Context Missing

```
# Workflow Status

No active tasks.

## Project Context
Status: ⚠ Not Found

⚡ Performance Tip:
Create project context to speed up all workflows by 40-60%.
One-time setup takes 2-3 minutes.

Run: /init-context

## Recent Activity
[Show last session if session-log exists]

Ready for next task!
```

## Task Status Legend

| Icon | Status | Meaning |
|------|--------|---------|
| ✓ | completed | Task finished |
| ⚙ | in_progress | Currently working |
| ⏳ | pending | Not started (may be blocked) |
| 🔒 | blocked | Waiting for other tasks |

## When to Use

- Check progress during long workflows
- See which task is currently active
- Understand what's blocking progress
- Verify context is available before starting work
- Resume after session restart

## Technical Notes

- Uses native `TaskList` tool
- Tasks persist in `~/.claude/tasks/`
- Fast operation (<1 second)
- Read-only (no modifications)
- Shows real-time task state

---

**Quick health check using native Tasks.**
