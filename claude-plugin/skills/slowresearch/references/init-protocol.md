# Init Protocol

## When invoked: `/slowresearch`

Sets up a new experiment campaign. Creates config and results files.

## Protocol

### Step 1: Check for existing campaign

Read `slowresearch-config.json` in the current directory.

- If it exists → tell the user a campaign already exists. Show the goal and experiment count from the TSV. Ask if they want to continue the existing one or start fresh (which will archive the old files with a timestamp suffix).
- If it doesn't exist → proceed to setup.

### Step 2: Interactive setup

Use `AskUserQuestion` to gather campaign parameters. Batch into 2 rounds max.

**Batch 1 (3 questions):**

1. **Goal** — "What are you trying to optimize?"
   - Options: "Maximize engagement" / "Increase conversions" / "Improve response rate" / Other
   - The user should provide a specific, measurable goal

2. **Metrics** — "What metrics will you track? (list all that matter)"
   - Free text, comma-separated
   - Examples: "impressions, comments, shares" or "open_rate, reply_rate, meetings_booked"

3. **Context** — "Describe what you're experimenting with"
   - Free text
   - Examples: "LinkedIn posts about gaming industry" or "cold emails to SaaS founders"

**Batch 2 (2 questions):**

4. **Primary metric** — "Which metric matters most?"
   - Options dynamically generated from the metrics they provided in batch 1

5. **Direction** — "Is higher or lower better for your primary metric?"
   - Options: "Higher is better" / "Lower is better"

### Step 3: Create files

1. Write `slowresearch-config.json`:

```json
{
  "goal": "<from setup>",
  "metrics": ["<metric1>", "<metric2>", ...],
  "primary_metric": "<from setup>",
  "direction": "higher|lower",
  "context": "<from setup>",
  "created_at": "<today's date YYYY-MM-DD>"
}
```

2. Write `slowresearch-results.tsv` with header row only:

```
id\tdate\tdescription\thypothesis\t<metric1>\t<metric2>\t...\tstatus\tlearnings
```

Metric columns are dynamic, derived from the config.

3. Check `.gitignore` — if it exists, append `slowresearch-config.json`, `slowresearch-results.tsv`, and `slowresearch-current.md` if not already present. If no `.gitignore`, create one with these entries.

### Step 4: Confirm

Tell the user:
- Campaign created successfully
- Show the config summary
- Explain the 4 commands and when to use each
- Suggest running `/slowresearch:step` to get their first experiment recommendation, or `/slowresearch:log` if they've already run an experiment they want to record
