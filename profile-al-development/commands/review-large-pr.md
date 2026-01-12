---
description: Review a large PR using parallel agents. Output to .review/ directory.
argument-hint: "[branch-or-commit-range] e.g., 'main..HEAD' or 'feature-branch'"
allowed-tools: ["Task", "Bash", "Read", "Write", "Glob", "Grep"]
---

# Large PR Review Workflow

**Target:** `$ARGUMENTS`

All output goes to `.review/` directory. Agents return one-line status only.

## Step 1: Setup

```bash
mkdir -p .review
```

## Step 2: Scan (pr-scanner)

Launch `pr-scanner` agent with the commit range.

**Expected output:** `Scan complete → .review/01-scan.md`

## Step 3: Read Scan Results

```bash
cat .review/01-scan.md
```

Get the suggested chunks from the scan.

## Step 4: Parallel Chunk Analysis

Launch multiple `pr-chunk-analyzer` agents **in a single message**:

For each chunk from the scan:
- Agent prompt: "Analyze chunk N: [file list]. Write to .review/02-chunk-N.md"
- Pass the chunk number so files are named correctly

**Expected outputs:**
- `Chunk 1 complete → .review/02-chunk-1.md`
- `Chunk 2 complete → .review/02-chunk-2.md`
- etc.

## Step 5: Supporting Checks (parallel)

In the same message as chunk analyzers, also launch:
- `al-standards-checker` → `.review/03-standards.md`
- `al-compiler` → `.review/04-compiler.md`
- `dependency-analyzer` → `.review/05-dependencies.md`

## Step 6: Aggregate

After all agents complete, launch `review-aggregator`:
- Reads all `.review/*.md` files
- Writes final report to `.review/99-summary.md`

**Expected output:** `Review complete → .review/99-summary.md`

## Step 7: Report to User

```
Review complete. Files in .review/:
- 99-summary.md (start here)
- 01-scan.md
- 02-chunk-*.md
- 03-standards.md
- 04-compiler.md
- 05-dependencies.md
```

## Important

- Keep chat minimal - all details in files
- Launch agents in parallel where possible
- If >100 files, use more chunks (5-6 instead of 3)
- Check `.review/99-summary.md` for recommendation
