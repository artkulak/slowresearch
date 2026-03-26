# Report Protocol

## When invoked: `/slowresearch:report`

Comprehensive analysis of all experiments. Read-only — does not modify any files.

## Protocol

### Step 1: Load state

1. Read `slowresearch-config.json`. If missing → tell user to run `/slowresearch` first.
2. Read `slowresearch-results.tsv`. If no completed experiments → tell user to log some results first.

### Step 2: Generate report

Structure the report in these sections:

#### Campaign Overview

```
Goal: [from config]
Context: [from config]
Primary metric: [name] ([direction])
Experiments: [count] over [date range]
```

#### Summary Stats

For each metric:
- Best / Worst / Average / Median
- Trend direction (improving, declining, flat) based on last 5 vs first 5

For primary metric specifically:
- Current best result and which experiment produced it
- Running average of last 3 experiments vs overall average

#### Hypothesis Scorecard

For each unique hypothesis tested:

```
| Hypothesis | Times Tested | Avg [Primary] | Best [Primary] | Verdict |
```

Verdicts:
- **Confirmed** — tested 2+ times, consistently above average
- **Refuted** — tested 2+ times, consistently below average
- **Promising** — tested once, above average (needs confirmation)
- **Weak** — tested once, below average
- **Inconclusive** — mixed results across tests

#### Top Patterns

Identify the 3-5 strongest signals from the data:
- What consistently correlates with high performance?
- What consistently correlates with low performance?
- Any surprising findings?

Be specific. "Posts with game names" is better than "certain types of hooks."

#### Recommended Next 3 Experiments

Ranked by expected learning value:

1. **[Experiment]** — [why this is valuable now]
2. **[Experiment]** — [why this is valuable now]
3. **[Experiment]** — [why this is valuable now]

Mix exploitation (double down on winners) and exploration (test new territory).

#### Campaign Health

- **Plateau check** — are the last 5 experiments clustered in a narrow range? If so, suggest a radical change of direction.
- **Exploration vs exploitation** — what % of experiments tested new hypotheses vs confirmed existing ones? Suggest rebalancing if skewed.
- **Sample size warning** — flag any conclusions drawn from fewer than 3 data points.

### Step 3: Suggest next action

End with a recommendation:
- If there's a clear next experiment → "Run `/slowresearch:step` to get a specific recommendation"
- If the campaign seems stale → suggest revisiting the goal or adding new metrics
- If there's enough data for a strategy shift → describe the shift
