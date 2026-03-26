# Log Protocol

## When invoked: `/slowresearch:log`

Records results from a completed experiment. Appends to the results TSV.

## Protocol

### Step 1: Load state

1. Read `slowresearch-config.json`. If missing → tell user to run `/slowresearch` first.
2. Read `slowresearch-results.tsv`. Count existing rows to determine next ID.

### Step 2: Gather results

**Check what was provided in arguments first.** Parse inline args for:
- Description (quoted string or unkeyed text)
- `metric=value` pairs matching config metric names
- `date=YYYY-MM-DD` (optional, default: today)
- `hypothesis=...` (optional, default: empty)
- `learnings=...` (optional, default: empty)

**If ALL config metrics are present in args → skip interactive input entirely.**

If some metrics are missing, use `AskUserQuestion` to collect only the missing fields. Batch into 1 round max.

**Interactive fields (only ask for what's missing):**

1. **Description** — "What did you do? (brief description of the experiment)"
   - Free text
   - Example: "Posted with a specific game name in the hook"

2. **Hypothesis** — "What hypothesis were you testing? (optional — leave blank for baseline data)"
   - Free text, optional
   - Example: "Naming specific games in hooks increases engagement"
   - If `/slowresearch:step` was used, pre-fill with the hypothesis from the last recommendation

3. **Metrics** — One question per missing metric defined in config
   - "What was the [metric_name]?"
   - Numeric values

4. **Date** — "When did this happen? (default: today)"
   - YYYY-MM-DD format
   - Only ask if logging historical data and not provided in args

5. **Learnings** — "What did you learn? (optional — I can suggest learnings based on the data)"
   - Free text, optional

### Step 3: Append to TSV

Construct a new row:
- `id`: increment from last row
- `date`: from `date=` arg if provided, otherwise today's date (YYYY-MM-DD)
- `description`: from user
- `hypothesis`: from user, or empty string if not provided
- `<metrics>`: from user, one column per metric
- `status`: `completed`
- `learnings`: from user, or empty

Append to `slowresearch-results.tsv`.

### Step 4: Mini-analysis

After logging, automatically provide a brief analysis:

1. **Comparison to average** — "This result was [X% above/below] your average [primary_metric]"
2. **Hypothesis check** — only if hypothesis is non-empty:
   - "This [confirms/contradicts/is inconclusive for] the hypothesis '[hypothesis]'"
   - If this hypothesis has been tested before, compare results
   - If only 1 data point, say "One data point — needs confirmation"
   - If hypothesis is empty, skip this section entirely
3. **Ranking update** — "This experiment ranks [#N out of M] by [primary_metric]"
4. **Quick suggestion** — One sentence on what this means for the next experiment

End with: "Run `/slowresearch:step` for a detailed next experiment recommendation, or `/slowresearch:report` for a full analysis."

---

## Bulk Mode

When the user provides multiple data points at once (CSV, table, list, or multiple entries described in conversation), enter bulk mode:

1. **Parse all entries** from the user's input. Extract description, metrics, date, and hypothesis for each entry.
2. **Show a summary table** of what will be logged:
   ```
   | # | Date | Description | metric1 | metric2 | ... |
   ```
3. **Ask for confirmation once** — "Log these [N] entries? (y/n)"
4. **Append all rows** to the TSV in one operation, incrementing IDs sequentially.
5. **Run batch mini-analysis** instead of per-entry analysis:
   - Average, best, and worst for primary metric across the batch
   - Total entries logged
   - If enough data, identify any patterns in the batch

**Detection:** Enter bulk mode if:
- The user's input contains multiple newline-separated entries with metrics
- The user pastes tabular data (CSV, TSV, markdown table)
- The user says "log these" / "log all" / "import" with plural data
- The conversation context contains a list of experiments to log
