# Log Protocol

## When invoked: `/slowresearch:log`

Records results from a completed experiment. Appends to the results TSV.

## Protocol

### Step 1: Load state

1. Read `slowresearch-config.json`. If missing → tell user to run `/slowresearch` first.
2. Read `slowresearch-results.tsv`. Count existing rows to determine next ID.

### Step 2: Gather results

Use `AskUserQuestion` to collect experiment data. Batch into 1-2 rounds.

**Batch 1 (up to 4 questions):**

1. **Description** — "What did you do? (brief description of the experiment)"
   - Free text
   - Example: "Posted with a specific game name in the hook"

2. **Hypothesis** — "What hypothesis were you testing?"
   - Free text
   - Example: "Naming specific games in hooks increases engagement"
   - If `/slowresearch:step` was used, pre-fill with the hypothesis from the last recommendation

3. **Metrics** — One question per metric defined in config (up to 2 in this batch)
   - "What was the [metric_name]?"
   - Numeric values

**Batch 2 (if more than 2 metrics):**

4. **Remaining metrics** — continue collecting metric values

5. **Learnings** — "What did you learn? (optional — I can suggest learnings based on the data)"
   - Free text, optional

### Step 3: Append to TSV

Construct a new row:
- `id`: increment from last row
- `date`: today's date (YYYY-MM-DD)
- `description`: from user
- `hypothesis`: from user
- `<metrics>`: from user, one column per metric
- `status`: `completed`
- `learnings`: from user, or empty

Append to `slowresearch-results.tsv`.

### Step 4: Mini-analysis

After logging, automatically provide a brief analysis:

1. **Comparison to average** — "This result was [X% above/below] your average [primary_metric]"
2. **Hypothesis check** — "This [confirms/contradicts/is inconclusive for] the hypothesis '[hypothesis]'"
   - If this hypothesis has been tested before, compare results
   - If only 1 data point, say "One data point — needs confirmation"
3. **Ranking update** — "This experiment ranks [#N out of M] by [primary_metric]"
4. **Quick suggestion** — One sentence on what this means for the next experiment

End with: "Run `/slowresearch:step` for a detailed next experiment recommendation, or `/slowresearch:report` for a full analysis."
