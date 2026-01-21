---
description: Navigate BC base app to find objects and extension points
allowed-tools: ["Task"]
---

# Base App Navigation

Explore BC base app objects, find extension points, and understand dependencies.

## Usage

```
/nav-baseapp "Find all Customer validation events"
```

## What It Does

Spawns the **dependency-navigator** agent to explore base app and document findings.

## Output

- `.dev/nav-[topic].md` - Object definitions, events, relationships, extension points

## When to Use

- Need to find base app objects
- Looking for available events
- Want to understand table relationships
- Need to find extension points
- Exploring existing patterns

## Example Queries

- "Customer table events"
- "Sales posting flow"
- "Where is CreditLimit field used?"
- "Find posting events in Sales-Post codeunit"
