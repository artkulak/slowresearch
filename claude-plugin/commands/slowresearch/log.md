---
name: slowresearch:log
description: Record results from a completed experiment
argument-hint: "[description] [metric=value ...] [date=YYYY-MM-DD] [hypothesis=...]"
---

EXECUTE IMMEDIATELY — do not deliberate.

## Argument Parsing (do this FIRST)

Extract from arguments if provided:
- Quoted string or unkeyed text → description
- `metric=value` pairs (where metric matches a name from config) → metric values
- `date=YYYY-MM-DD` → experiment date (default: today)
- `hypothesis=...` → hypothesis text (default: empty)
- `learnings=...` → learnings text (default: empty)

## Execution

1. Read `skills/slowresearch/SKILL.md` for full context
2. Read `skills/slowresearch/references/log-protocol.md` for the log protocol
3. Read `slowresearch-config.json` from the current directory
4. Read `slowresearch-results.tsv` from the current directory
5. If ALL config metrics are provided in arguments → skip `AskUserQuestion`, proceed directly
6. If some fields missing → use `AskUserQuestion` only for missing metrics
7. If the user provides multiple data points (CSV, table, list, or multiple entries in conversation) → enter bulk mode per the log protocol
8. Append new row(s) to `slowresearch-results.tsv`
9. Provide mini-analysis comparing this result to history
