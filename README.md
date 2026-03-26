# slowresearch

Human-in-the-loop experiment iteration for domains with delayed, real-world feedback.

The deliberate counterpart to [autoresearch](https://github.com/karpathy/autoresearch). Same loop — **hypothesis → experiment → measure → learn → repeat** — but designed for domains where feedback takes hours/days/weeks and actions can't be undone.

## Why

autoresearch works because verification is instant: run test → get score → keep or revert. But most real-world optimization doesn't work that way:

- You publish a LinkedIn post and wait 48 hours for engagement data
- You send a cold email sequence and wait 3 days for reply rates
- You change your pricing page and wait 2 weeks for conversion data

You can't run 100 experiments overnight. You can't revert a published post. But you *can* learn systematically from each experiment and make the next one better.

That's slowresearch.

## Install

```bash
claude plugin marketplace add artkulak/slowresearch
claude plugin install slowresearch
```

### Update

```bash
claude plugin marketplace update slowresearch
claude plugin update "slowresearch@slowresearch"
```

Restart Claude Code after updating.

## Commands

| Command | What it does |
|---------|-------------|
| `/slowresearch` | Set up a new experiment campaign |
| `/slowresearch:step` | Get a recommendation for your next experiment |
| `/slowresearch:log` | Record results from a completed experiment |
| `/slowresearch:report` | Full analysis with hypothesis scorecard and recommendations |

## How It Works

```
/slowresearch                    # Define your goal, metrics, context
  ↓
/slowresearch:step               # "Try X to test hypothesis Y"
  ↓
  ... do the thing, wait for results ...
  ↓
/slowresearch:log                # Record what happened
  ↓
/slowresearch:step               # "Based on 3 experiments, try Z next"
  ↓
  ... repeat for weeks/months ...
  ↓
/slowresearch:report             # "Here's what works, what doesn't, what to try next"
```

## Examples

### LinkedIn Content

```
> /slowresearch
Goal: Maximize post engagement
Metrics: impressions, comments, engagement_rate
Context: LinkedIn posts about gaming industry, 2-3x per week

> /slowresearch:step
Next experiment: Post with a specific game name in the hook instead of a generic industry reference.
Hypothesis: Naming specific games increases engagement because it triggers recognition and opinion.

  ... post goes live, wait 48h ...

> /slowresearch:log "Roblox economy post" impressions=8200 comments=12 engagement_rate=0.15
→ This is 3x your average impressions. Confirms the hypothesis — naming games works.

> /slowresearch:step
Next experiment: Test whether the game name effect works for lesser-known games too.
Hypothesis: The engagement boost comes from recognition, not just naming any game.
```

### Cold Outreach

```
> /slowresearch
Goal: Increase meeting booking rate
Metrics: open_rate, reply_rate, meetings_booked
Context: Cold emails to SaaS founders, 50 emails/week

> /slowresearch:step
Next experiment: Lead with a specific observation about the prospect's product instead of a generic pain point.
Hypothesis: Personalized observations increase reply rates because they demonstrate research effort.
```

### Pricing Experiments

```
> /slowresearch
Goal: Maximize conversion rate
Metrics: page_views, trial_starts, conversion_rate, revenue
Context: SaaS pricing page, ~500 visitors/week

> /slowresearch:step
Next experiment: Remove the enterprise tier and add a "Contact us" link instead.
Hypothesis: Fewer choices reduce decision paralysis and increase trial starts.
```

## How It Differs From autoresearch

| | autoresearch | slowresearch |
|---|---|---|
| **Loop** | Autonomous — runs forever | Human-triggered steps |
| **Verification** | Shell command (instant) | Human provides metrics |
| **Revert** | `git revert` on failure | No revert — learn forward |
| **Metric** | Single number | Multiple metrics, one primary |
| **Hypotheses** | Implicit in commits | First-class, tracked and scored |
| **Session** | One long-running session | Across days/weeks/months |
| **Best for** | Code, ML, benchmarks | Content, outreach, pricing, hiring |

## State Files

slowresearch creates three gitignored files in your project:

- **`slowresearch-config.json`** — campaign goal, metrics, and context
- **`slowresearch-results.tsv`** — experiment log with metrics and learnings
- **`slowresearch-current.md`** — current in-progress experiment (written by `/step`, cleared by `/log`)

## Use Cases

Any domain where you: take an action → wait for real-world results → can't undo it → want to learn for next time.

- **Content marketing** — blog posts, LinkedIn, newsletters, tweets
- **Cold outreach** — email sequences, LinkedIn DMs, cold calls
- **Pricing** — plans, tiers, discounts, trials
- **Hiring** — job postings, interview processes, offers
- **Product** — onboarding flows, feature launches, A/B tests
- **SEO** — content, meta descriptions, link building
- **Ads** — copy, creative, targeting, bidding

## License

MIT
