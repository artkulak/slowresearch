---
name: slowresearch:log
description: Record results from a completed experiment
argument-hint: ""
---

EXECUTE IMMEDIATELY — do not deliberate.

## Execution

1. Read `skills/slowresearch/SKILL.md` for full context
2. Read `skills/slowresearch/references/log-protocol.md` for the log protocol
3. Read `slowresearch-config.json` from the current directory
4. Read `slowresearch-results.tsv` from the current directory
5. Use `AskUserQuestion` to gather experiment description, hypothesis, metric values, and learnings
6. Append new row to `slowresearch-results.tsv`
7. Provide mini-analysis comparing this result to history
