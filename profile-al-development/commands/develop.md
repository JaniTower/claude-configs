---
description: Run development phase only (implement → review → fix diagnostics) with iteration loops
allowed-tools: ["Task", "Read"]
---

# Development Phase

Run only the development phase with automatic iteration loops for quality.

## Usage

```
/develop
```

**Prerequisite:** Must have `.dev/02-solution-plan.md` from planning phase.

## What It Does

Runs development agents with iteration loops:

### Initial Implementation
1. **Spawn al-developer** - Implement AL code from plan

### Code Review Loop (Iterative)
2. **Spawn code-reviewer** - Review code quality
3. **Evaluate findings:**
   - **If Critical/High issues:** ITERATE back to al-developer → Go to step 2
   - **If only Medium/Low issues:** Continue to step 4

### Compilation Loop (Iterative)
4. **Spawn diagnostics-fixer** - Fix auto-fixable issues
5. **Evaluate compilation:**
   - **If complex errors (3+ or logic issues):** ITERATE back to al-developer → Go to step 2
   - **If minor/no errors:** Done

## Iteration Logic

### Code Review Triggers Iteration When:
- **Critical issues found** (security, crashes, compliance)
- **High priority issues found** (performance, DRY violations, missing functionality)

### Diagnostics Fixer Triggers Iteration When:
- **3+ complex compiler errors** remain after auto-fixing
- **Logic errors** (type mismatches requiring business decisions)
- **Missing declarations** (ambiguous fixes)
- **Breaking changes** (API modifications)

### No Iteration When:
- Only Medium/Low code review issues (documentation, minor style)
- Only 1-2 trivial compiler errors (typos, semicolons)
- Only warnings (CodeCop style issues)

## Output Files

- `.dev/04-code-review.md` - Code quality assessment
- `.dev/05-diagnostics.md` - Compilation diagnostics
- AL code files created/modified (in `src/`)
- `.dev/session-log.md` - Updated with all iterations

## Expected Duration

- **Initial implementation:** ~5-10 min
- **Code review:** ~3-5 min per iteration
- **If iteration needed:** +5-10 min per loop (typically 0-2 loops)
- **Diagnostics fixing:** ~2-5 min

**Total:** 10-30 minutes (depending on code quality and iterations)

## When to Use

- Have implementation plan ready (`.dev/02-solution-plan.md`)
- Want to implement existing plan with quality checks
- Re-implementing after design changes
- Continuing from `/plan` command

## Iteration Examples

### Example 1: Clean Implementation (No Iteration)
```
al-developer runs → code complete
code-reviewer runs → Only Medium/Low issues (documentation)
diagnostics-fixer runs → Clean compilation
✓ Done in 12 minutes
```

### Example 2: Single Iteration (Critical Bug)
```
al-developer runs → code complete
code-reviewer runs → 1 Critical issue (missing error handling)
  ↻ ITERATE: al-developer fixes error handling
code-reviewer runs → Only Low issues
diagnostics-fixer runs → Clean compilation
✓ Done in 18 minutes (1 iteration)
```

### Example 3: Two Iterations (Complex Errors)
```
al-developer runs → code complete
code-reviewer runs → 2 High issues (performance, DRY)
  ↻ ITERATE: al-developer fixes performance + DRY
code-reviewer runs → All clear
diagnostics-fixer runs → 5 type mismatch errors
  ↻ ITERATE: al-developer fixes type errors
code-reviewer runs → All clear
diagnostics-fixer runs → Clean compilation
✓ Done in 27 minutes (2 iterations)
```

## What Gets Fixed Where

| Issue Type | Handler | Iteration? |
|------------|---------|------------|
| Missing error handling | code-reviewer detects → al-developer fixes | Yes |
| DRY violations | code-reviewer detects → al-developer fixes | Yes |
| Performance issues | code-reviewer detects → al-developer fixes | Yes |
| Type mismatches | diagnostics-fixer detects → al-developer fixes | Yes (if complex) |
| Logic errors | diagnostics-fixer detects → al-developer fixes | Yes |
| Missing spaces | diagnostics-fixer auto-fixes | No |
| Missing parentheses | diagnostics-fixer auto-fixes | No |
| Missing documentation | code-reviewer documents → diagnostics-fixer may add | No |
| Style warnings | diagnostics-fixer auto-fixes | No |

## Next Steps

After development completes:
1. Review `.dev/04-code-review.md` for any remaining Medium/Low issues
2. Review `.dev/05-diagnostics.md` for compilation status
3. Run `/test` to create comprehensive tests

---

**Note:** The iteration loops ensure high-quality code before proceeding to testing, catching issues early when they're easier to fix.
