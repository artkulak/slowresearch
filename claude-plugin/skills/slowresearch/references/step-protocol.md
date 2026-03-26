# Step Protocol

## When invoked: `/slowresearch:step`

Proposes the next experiment based on accumulated history. Read-only — does not modify any files.

## Protocol

### Step 1: Load state

1. Read `slowresearch-config.json`. If missing → tell user to run `/slowresearch` first.
2. Read `slowresearch-results.tsv`. If missing or empty (header only) → this is the first experiment.

### Step 2: Analyze history

If this is the **first experiment** (no results):
- Propose a baseline experiment or an initial hypothesis to test
- Keep it simple — the first experiment establishes a reference point
- Suggest what to pay attention to when recording results

If there are **1-5 experiments**:
- Summarize what's been tried
- Identify which hypotheses have been tested
- Note any early patterns (but caveat: small sample size)
- Prioritize: explore broadly, don't exploit yet

If there are **6-15 experiments**:
- Calculate averages and ranges for each metric
- Rank hypotheses by primary metric performance
- Identify the top 2-3 patterns
- Start suggesting exploitation of what's working + one exploration bet
- Flag any hypotheses with only 1 data point (need confirmation)

If there are **16+ experiments**:
- Full statistical summary (mean, best, worst, trend direction)
- Hypothesis scorecard (confirmed/refuted/inconclusive)
- Identify diminishing returns if recent experiments cluster around similar results
- Recommend: double down on confirmed winners, drop refuted approaches, try one radical new angle
- Explicitly state whether the campaign is in exploration or exploitation mode

### Step 3: Propose ONE experiment

Output a clear recommendation:

```
## Next Experiment

**What to do:** [specific, actionable description]

**Hypothesis:** [what this tests — stated as a falsifiable claim]

**Expected outcome:** [based on history, what you predict will happen]

**Why this is the highest-value next test:** [reasoning — what makes this better than alternatives]
```

### Step 4: Remind

End with: "Run `/slowresearch:log` after you have results to record."

## Priority Order for Choosing Next Experiment

1. **Confirm a promising pattern** — if something worked once, verify it wasn't a fluke
2. **Exploit confirmed winners** — combine multiple things that work
3. **Explore untested territory** — try something no prior experiment has covered
4. **Refine a near-miss** — if something almost worked, try a variation
5. **Challenge assumptions** — try the opposite of what's been working (every 5-10 experiments)
6. **Simplify** — if complexity is creeping in, test whether a simpler approach works just as well
