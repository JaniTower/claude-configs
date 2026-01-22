---
description: Run testing phase only (create tests → review tests)
allowed-tools: ["Task"]
---

# Testing Phase

Run only the testing phase: test creation and test review.

## Usage

```
/test
```

**Prerequisite:** Must have implemented code and `.dev/02-solution-plan.md`.

## What It Does

Runs testing agents in sequence:
1. **test-engineer** - Create comprehensive tests
2. **test-reviewer** - Review test coverage

## Output

- `.dev/06-test-plan.md`
- `.dev/07-test-review.md`
- Test codeunits (in src/tests/)

## When to Use

- Code implementation complete
- Want comprehensive test coverage
- Need to verify requirements met

## Next Steps

After testing:
- Review test review report
- Run manual UI tests
- Deploy to test environment
