---
name: slowresearch
description: Set up a new experiment campaign for human-in-the-loop iteration with delayed feedback
argument-hint: "[Goal: <goal>] [Metrics: <metrics>] [Context: <context>]"
---

EXECUTE IMMEDIATELY — do not deliberate.

## Argument Parsing (do this FIRST)

Extract from arguments if provided:
- `Goal:` — what to optimize
- `Metrics:` — comma-separated metric names
- `Context:` — what the user is experimenting with

## Execution

1. Read `skills/slowresearch/SKILL.md` for full context
2. Read `skills/slowresearch/references/init-protocol.md` for the setup protocol
3. If all fields provided in arguments → skip interactive setup, proceed directly
4. If any fields missing → use `AskUserQuestion` as specified in the protocol
5. Create config and results files
6. Confirm setup and explain available commands
