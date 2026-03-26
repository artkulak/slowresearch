# Core Principles

## How slowresearch differs from autoresearch

slowresearch shares autoresearch's core loop (hypothesis → experiment → measure → learn → repeat) but adapts it for real-world domains with delayed feedback.

### 1. Human-Paced, Not Machine-Paced

autoresearch runs 100 experiments overnight. slowresearch runs 1 experiment per day/week. Each experiment matters more. Recommendations must be high-quality because iteration cost is high.

### 2. No Revert — Learn Forward

You can't unpublish a LinkedIn post or unsend a cold email. Every experiment is permanent. Instead of "keep or revert," slowresearch asks "what did we learn?" Failed experiments are valuable data, not wasted iterations.

### 3. Multiple Metrics, One Primary

Real-world experiments produce multiple signals (impressions AND comments AND engagement rate). slowresearch tracks all of them but optimizes for one primary metric. Secondary metrics provide context and catch regressions.

### 4. Hypotheses Are First-Class

In autoresearch, hypotheses are implicit in commit messages. In slowresearch, every experiment explicitly states what hypothesis it tests. Hypotheses accumulate a track record: confirmed, refuted, or inconclusive. This is the main learning mechanism.

### 5. State Persists Across Sessions

autoresearch runs in one long session. slowresearch spans days or weeks across many sessions. State lives in files (config JSON + results TSV), not in conversation context or git history.

### 6. Compound Learning

The agent reads ALL prior experiments before proposing the next one. Patterns emerge over time. Early experiments explore broadly; later experiments exploit what's working. The agent should explicitly call out when it shifts from exploration to exploitation.

### 7. Honest Uncertainty

With 3 data points, you can't draw conclusions. The agent must be honest about statistical significance. "This worked once" is not "this always works." Recommend confirmation experiments when sample sizes are small.
