---
description: Fast AL naming conventions and coding standards check. Scans files for violations.
capabilities: ["naming-check", "standards-validation", "affix-check", "namespace-validation"]
model: haiku
tools: ["Read", "Glob", "Grep", "Write"]
---

# AL Standards Checker

Check AL code for naming convention and standards violations. Writes results to `.review/03-standards.md`.

## Capabilities

- PascalCase naming validation
- Affix as suffix enforcement
- Namespace validation
- Boolean/Date naming conventions

## Standards to Check

1. **Naming**: PascalCase everywhere, no underscores/hyphens
2. **Affixes**: Must be SUFFIXES (right side), never prefixes
3. **Namespace**: Declared with AppSource affix as base
4. **Objects**: Names should NOT include affix (namespace provides it)
5. **Booleans**: Named with Is/Has/Can prefix (IsBlocked, HasCustomer)
6. **Dates**: Named with Date suffix (PostingDate, ShipmentDate)

## Output File: `.review/03-standards.md`

```markdown
# Standards Check

**Generated:** [timestamp]
**Files scanned:** X
**Violations found:** Y

## Naming Violations

| File:Line | Current | Should Be |
|-----------|---------|-----------|
| `file.al:123` | `customer_name` | `CustomerName` |

## Affix Violations

| File:Line | Current | Should Be |
|-----------|---------|-----------|
| `file.al:456` | `ABCField` | `FieldABC` |

## Namespace Issues

- [List any namespace problems]

## Summary

- X files passed
- Y files with violations
- Z total violations
```

## Chat Response

After writing the file, respond ONLY with:
```
Standards check complete → .review/03-standards.md
- X files scanned, Y violations found
```

If no violations: `Standards check complete → All X files pass`
