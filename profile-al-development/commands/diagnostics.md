---
description: Run AL compiler diagnostics and auto-fix safe issues (CodeCop, spacing, etc.)
allowed-tools: ["Task"]
---

# Diagnostics

Run AL compiler with all analyzers, auto-fix safe issues, report complex issues.

## Usage

```
/diagnostics
```

**No arguments needed** - runs on current project directory.

## What It Does

Spawns **diagnostics-fixer** agent directly:
1. **Compile** with all analyzers (CodeCop, UICop, AppSourceCop, LinterCop)
2. **Auto-fix safe issues:**
   - Missing spaces/parentheses
   - Unnecessary BEGIN..END blocks
   - Missing XML documentation
   - CodeCop formatting
3. **Report complex issues:**
   - Type mismatches
   - Undeclared identifiers
   - Logic errors
   - Breaking changes
4. **Recompile** to verify fixes
5. **Write report** → `.dev/05-diagnostics.md`

## Output

- `.dev/05-diagnostics.md` - Full diagnostics report
- Modified AL files (auto-fixed issues)
- `.dev/session-log.md` - Updated

## When to Use

**✓ Good for:**
- Checking existing code for errors/warnings
- Cleaning up CodeCop violations
- Pre-commit quality check
- After manual code changes
- Verifying compilation after merge

**✗ Not for:**
- New feature development (use `/develop` or `/dev-cycle`)
- Complex bug fixes (use `/fix`)
- Code review (included in `/develop`)

## Expected Results

### Best Case: Clean Compilation
```
Diagnostics complete → .dev/05-diagnostics.md

Summary:
- Initial: 8 warnings
- Auto-fixed: 8 issues (spacing, parentheses)
- Final: Clean compilation ✓

All issues resolved automatically.
```

### Common Case: Some Issues Fixed
```
Diagnostics complete → .dev/05-diagnostics.md

Summary:
- Initial: 3 errors, 12 warnings
- Auto-fixed: 12 warnings
- Remaining: 3 errors (manual review required)

Complex issues need attention:
- Type mismatch in CustomerProcessor.al:45
- Undeclared identifier in SalesPost.al:123
- Missing table relation in CustomerExt.al:67
```

### Worst Case: Complex Errors
```
Diagnostics complete → .dev/05-diagnostics.md

Summary:
- Initial: 15 errors, 5 warnings
- Auto-fixed: 5 warnings
- Remaining: 15 errors (significant issues)

Recommendation: Use /fix or /develop to address logic errors
```

## What Gets Auto-Fixed

| Issue Code | Description | Auto-Fixed? |
|------------|-------------|-------------|
| AA0001 | Missing parentheses | ✓ Yes |
| AA0005 | Unnecessary BEGIN..END | ✓ Yes |
| AA0206 | Missing space after comma | ✓ Yes |
| AA0231 | Missing space around operators | ✓ Yes |
| AA0137 | Missing XML documentation | ✓ Yes |
| AA0470 | Placeholder function issues | ✓ Yes |
| AL0132 | Undeclared identifier | ❌ Manual |
| AL0254 | Type mismatch | ❌ Manual |
| AL0185 | Invalid property value | ❌ Manual |

## Comparison to Other Commands

| Feature | /diagnostics | /fix | /develop |
|---------|--------------|------|----------|
| Compiles code | ✓ | ✓ | ✓ |
| Auto-fixes safe issues | ✓ | ✓ | ✓ |
| Fixes complex issues | ❌ Reports only | ✓ | ✓ |
| Code review | ❌ | ❌ | ✓ |
| Implementation plan | ❌ | ❌ | ✓ |
| Duration | ~2-5 min | ~5-10 min | ~15-30 min |

## Example Workflow

### Scenario: Pre-commit Check
```bash
# Made manual changes to code
# Want to verify compilation before commit

/diagnostics

# Review .dev/05-diagnostics.md
# If clean → commit
# If issues → fix manually or use /fix
```

### Scenario: After Code Review Feedback
```bash
# Reviewer said "fix CodeCop warnings"

/diagnostics

# Auto-fixes all CodeCop issues
# Clean compilation → ready to commit
```

### Scenario: After Merge Conflicts
```bash
# Resolved merge conflicts manually
# Need to verify everything compiles

/diagnostics

# Checks for compilation errors
# Reports if anything broke during merge
```

## Integration with Workflow

```
Manual code changes
    ↓
/diagnostics → Auto-fix safe issues
    ↓
If complex errors remain:
    → Use /fix for targeted fixes
    → Or manually fix issues
    ↓
/diagnostics again → Verify clean
    ↓
Commit ✓
```

## Notes

- **Non-destructive:** Only auto-fixes safe, well-understood issues
- **Idempotent:** Safe to run multiple times
- **Fast:** Typically 2-5 minutes
- **Detailed report:** Shows before/after for every fix

---

**Quick tip:** Run `/diagnostics` before every commit to catch issues early!
