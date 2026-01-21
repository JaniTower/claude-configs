---
description: Interview user in-depth about feature requirements (40+ questions) - refined spec as output
argument-hint: "[optional: file path to existing spec]"
allowed-tools: ["Task", "Read", "Write", "AskUserQuestion"]
---

# Interview Command

Conduct a thorough technical interview to extract complete implementation details for BC/AL features.

## Usage

```bash
# Start fresh interview (creates .dev/00-interview.md)
/interview

# Interview about existing spec
/interview .dev/01-requirements.md
/interview docs/customer-validation-spec.md
```

## What This Does

Spawns the **interview agent** to ask 40+ deep technical questions across 11 categories:

1. **Business Logic** - Process flow, validation rules, permissions
2. **BC Integration** - Table/page extensions, event subscribers
3. **Data Model** - Fields, relations, keys, upgrade
4. **User Interface** - Layout, actions, visibility
5. **Error Handling** - Validation timing, messages, overrides
6. **Integration** - APIs, web services, imports
7. **Performance** - Scale, optimization, batch processing
8. **Testing** - Scenarios, test data, automation
9. **Security** - Permissions, audit, compliance
10. **Deployment** - Rollout, migration, rollback
11. **Edge Cases** - Boundaries, concurrency, unknowns

## Arguments

- **No argument**: Create new interview spec at `.dev/00-interview.md`
- **File path**: Interview about and refine existing spec file

## Output

**For new interview**: Creates `.dev/00-interview.md` with:
- All technical decisions captured
- Edge cases documented
- Acceptance criteria defined
- Next steps suggested

**For existing file**: Rewrites file with:
- Interview insights embedded
- Specific technical details
- Expanded edge cases
- Clear acceptance criteria

## Workflow

```
/interview
    ↓
[Agent asks 40+ questions via AskUserQuestion tool]
    ↓
[Captures decisions, edge cases, technical details]
    ↓
Writes refined spec → .dev/00-interview.md (or specified file)
    ↓
User can then run: /plan
```

## Example Session

```
User: /interview

Claude: I'll interview you about this feature to extract complete implementation details.

[AskUserQuestion: Business process questions]
User: [Answers]

[AskUserQuestion: Data model questions]
User: [Answers]

[AskUserQuestion: BC integration questions]
User: [Answers]

... (continues for 40+ questions across 11 categories)

Claude: Interview complete → .dev/00-interview.md

Summary:
- 47 questions asked
- 8 key technical decisions captured
- 12 edge cases identified
- 15 acceptance criteria defined

Next step: /plan to generate requirements → design → implementation
```

## When to Use

Use `/interview` when:
- **Starting a new feature** - Extract complete requirements upfront
- **Refining vague requirements** - User knows what they want but hasn't thought through details
- **Clarifying a spec** - Existing spec has gaps or ambiguities
- **Before planning** - Want to ensure all edge cases considered before design

Use `/plan` directly when:
- **Requirements are crystal clear** - No ambiguity, all edge cases known
- **Small, well-defined change** - Adding a single field with obvious behavior

## Integration with Development Cycle

```
/interview → .dev/00-interview.md
    ↓
/plan → requirements (01) → design (02) → implementation plan (03)
    ↓
/develop → AL code written
    ↓
/test → test plan (06) + test code
```

## Tips

- **Be patient** - 40+ questions is normal for complex features
- **Think through each question** - Quality answers = better implementation
- **Use "Other" option** - Add context beyond provided options
- **Surface unknowns** - "I don't know" is a valid answer that helps identify research needs

---

**Remember:** 30 minutes in interview saves hours in re-work later.
