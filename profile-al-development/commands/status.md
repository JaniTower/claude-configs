# Command: /status

Show current workflow state, progress, and project context status.

## Purpose

Quick overview of:
- Current workflow phase (if any workflow active)
- Completed phases
- Project context status
- Recent agent activity
- Pending approvals

## Implementation

### Step 1: Check Workflow State

Look for `.dev/workflow-state.json`:

```json
{
  "workflow_id": "uuid",
  "workflow_type": "dev-cycle",
  "started": "2026-01-22T10:30:00Z",
  "current_phase": "development",
  "completed_phases": ["requirements", "solution-planning"],
  "pending_approval": false,
  "last_agent": "al-developer",
  "iteration_count": 2,
  "documents_created": [
    ".dev/01-requirements.md",
    ".dev/02-solution-plan.md"
  ]
}
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

## Current Workflow: Development Cycle
Started: 2026-01-22 10:30:00 (45 minutes ago)

Progress:
✓ Requirements Analysis (01-requirements.md)
✓ Solution Planning (02-solution-plan.md)
⚙ Development (in progress - iteration 2)
  └─ Last: al-developer (5 min ago)
⏳ Code Review (pending)
⏳ Diagnostics & Testing (pending)

## Project Context
Status: ✓ Available (.dev/project-context.md)
Last Updated: 2026-01-22 09:15:00
Objects: 12 tables, 8 pages, 15 codeunits
Base App Points: 5 integrations documented

## Recent Activity (last 5 agents)
1. [14:25] al-developer - Implemented validation logic (iteration 2)
2. [14:18] code-reviewer - Found 3 medium issues, sent back
3. [14:12] al-developer - Initial implementation
4. [14:05] solution-planner - Created solution plan
5. [14:00] requirements-engineer - Analyzed requirements

## Next Steps
→ Waiting for al-developer to complete fixes
→ Then: code-reviewer will review again
→ User approval needed after code review passes

---

💡 Tip: Project context saves 5-15 min per workflow. Keep it updated!
```

### If No Active Workflow

```
# Workflow Status

No active workflow.

## Project Context
Status: ✓ Available (.dev/project-context.md)
Last Updated: 2026-01-22 09:15:00
Objects: 12 tables, 8 pages, 15 codeunits

## Last Completed Workflow
Type: Quick Fix (/fix)
Completed: 2026-01-22 13:45:00
Files Changed: 1
Duration: 4 minutes

## Available Commands
- /dev-cycle "description" - Full development cycle
- /fix "bug description" - Quick bug fix (2-5 min)
- /plan "feature" - Planning only
- /develop - Development phase
- /test - Testing phase

Ready for next task!
```

### If Context Missing

```
# Workflow Status

No active workflow.

## Project Context
Status: ⚠ Not Found

⚡ Performance Tip:
Create project context to speed up all workflows by 40-60%.
One-time setup takes 2-3 minutes.

Run: /init-context

This documents your project structure so agents don't need to explore every time.

## Recent Activity
[Show last session if session-log exists]

Ready for next task!
```

## When to Use

- Check progress during long workflows
- Verify context is available before starting work
- See what last agent did
- Understand where workflow is in pipeline

## Technical Notes

- Fast operation (<1 second)
- Read-only (no file modifications)
- Can run anytime without disrupting workflow
- Helps user understand what's happening

---

**Quick health check for your development session.**
