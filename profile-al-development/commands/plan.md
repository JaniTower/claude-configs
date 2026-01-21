---
description: Run planning phase only (requirements → solution plan) with approval gates
allowed-tools: ["Task", "Read", "AskUserQuestion"]
---

# Planning Phase

Run only the planning phase with approval gates between each step.

## Usage

```
/plan "Add customer credit limit validation feature"
```

## What It Does

Runs the two planning agents with approval gates:
1. Requirements engineering
2. Solution planning (architecture + implementation)

## Workflow with Approval Gates

### Step 1: Requirements Analysis

1. **Spawn requirements-engineer** with user's request
2. **Show `.dev/01-requirements.md` summary**
3. **🛑 APPROVAL GATE:** Ask user:
   - ✅ Approve and continue to solution plan
   - 🔄 Refine requirements
   - ❌ Stop here

### Step 2: Solution Planning

4. **Spawn solution-planner** (reads requirements, uses MCP tools)
5. **Show `.dev/02-solution-plan.md` summary**
6. **✅ Done!** Planning complete

## Critical Rules

### ALWAYS Stop and Ask

After requirements:
1. **Read the output file**
2. **Summarize key points** (3-5 bullets)
3. **Use AskUserQuestion** to get approval
4. **Only proceed if approved**

### Approval Question Format

```json
{
  "questions": [{
    "question": "Review .dev/01-requirements.md. Ready to proceed to solution planning?",
    "header": "Next Step",
    "multiSelect": false,
    "options": [
      {
        "label": "Approve - Continue",
        "description": "Looks good, proceed to solution planning"
      },
      {
        "label": "Refine - Need changes",
        "description": "I'll provide feedback to improve"
      },
      {
        "label": "Stop here",
        "description": "End the planning workflow"
      }
    ]
  }]
}
```

## Output

Planning documents in `.dev/`:
- `.dev/01-requirements.md`
- `.dev/02-solution-plan.md`
- `.dev/session-log.md`

## After Planning

User can then:
- Review solution plan carefully
- Proceed to `/develop` to implement the plan
- Run `/test` after development
- Or start over with feedback

## When to Use

- Want to plan before committing to implementation
- Need to review and approve approach first
- Creating documentation for approval
- Exploring different design options

## Example

```
User: /plan "Add email validation"

Claude: Spawning requirements-engineer...
[Agent runs]

Claude: Requirements complete → .dev/01-requirements.md

Summary:
- Email format validation
- Block invalid on save
- User-friendly errors

[AskUserQuestion: Approve / Refine / Stop]

User: [Approves]

Claude: Spawning solution-planner...
[Agent runs]

Claude: Solution plan complete → .dev/02-solution-plan.md

Architecture summary:
- Table extension approach
- Validation codeunit
- Event subscriber pattern

Implementation summary:
- 4 files to create
- 3 implementation phases
- Estimated 60 minutes

Ready to implement? Run: /develop
```

---

**Remember:** Stop after requirements for user approval before continuing to solution planning.
