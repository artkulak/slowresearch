# slowresearch

Human-in-the-loop experiment iteration for domains with delayed, real-world feedback.

## What This Is

slowresearch is the deliberate counterpart to [autoresearch](https://github.com/karpathy/autoresearch). While autoresearch runs autonomously with instant mechanical verification (run test → get score → keep/revert), slowresearch is designed for domains where:

- **Feedback is delayed** — hours, days, or weeks (not seconds)
- **Actions are irreversible** — you can't unpublish a post or unsend an email
- **Metrics are composite** — multiple numbers matter, not just one
- **Learning compounds across sessions** — experiments span days/weeks, not one sitting

The loop is the same: **hypothesis → experiment → measure → learn → repeat**. But verification is manual, there's no revert, and the agent learns forward instead of rolling back.

## Commands

| Command | Purpose |
|---------|---------|
| `/slowresearch` | Set up a new experiment campaign |
| `/slowresearch:step` | Propose the next experiment based on history |
| `/slowresearch:log` | Record results from a completed experiment |
| `/slowresearch:report` | Comprehensive analysis of all experiments |

## State Files

slowresearch uses two local files (gitignored by default):

### `slowresearch-config.json`

```json
{
  "goal": "Maximize LinkedIn post engagement",
  "metrics": ["impressions", "comments", "engagement_rate"],
  "primary_metric": "comments",
  "direction": "higher",
  "context": "LinkedIn posts about gaming industry, 2-3x per week",
  "created_at": "2026-03-26"
}
```

### `slowresearch-results.tsv`

Tab-separated, dynamic columns based on config metrics:

```
id	date	description	hypothesis	impressions	comments	engagement_rate	status	learnings
1	2026-03-26	Hook: named specific game	naming games increases engagement	8200	12	0.15	completed	Confirmed - 3x avg impressions
```

Valid statuses: `completed`, `in_progress`, `skipped`

## Command Routing

When the user invokes a slowresearch command:

1. **Read this file** (SKILL.md) for context
2. **Read the relevant reference file** for the specific protocol:
   - `/slowresearch` → `references/init-protocol.md`
   - `/slowresearch:step` → `references/step-protocol.md`
   - `/slowresearch:log` → `references/log-protocol.md`
   - `/slowresearch:report` → `references/report-protocol.md`
3. **Execute the protocol** as specified in the reference file

## Critical Rules

1. **NEVER fabricate metrics.** Only the human provides real-world results.
2. **NEVER modify the results TSV outside of `/slowresearch:log`.** The log is the source of truth.
3. **ONE experiment per step.** Each `/slowresearch:step` proposes exactly one next experiment.
4. **Hypotheses are first-class.** Every new experiment should state what hypothesis it tests. Historical/baseline entries may omit it.
5. **Learn forward, never revert.** There is no rollback. Failed experiments produce learnings, not reverts.
6. **Read before recommending.** Always read the full results TSV before proposing the next step.
7. **Config is immutable during a campaign.** Don't modify `slowresearch-config.json` after init unless the user explicitly asks.
