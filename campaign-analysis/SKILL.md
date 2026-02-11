---
name: campaign-analysis
description: This skill should be used when the user invokes "campaign-analysis" directly, or asks to "analyze a GOAL client campaign", "diagnose campaign performance", "generate bid recommendations", "review campaign data", "optimize bid multipliers", "analyze source performance", "run a campaign analysis", or when the user uploads GOAL Platform campaign CSVs and asks for performance analysis or bid optimization recommendations. Also triggers when the user mentions RightPricing calibration, source-level diagnosis, win rate analysis, or bid rate issues for a GOAL client.
---

# GOAL Platform Campaign Analysis

Systematic, multi-layer analytical framework for diagnosing GOAL Platform client campaign performance and generating calibrated bid optimization recommendations. This framework operates across four analytical layers, each building on the previous.

## Overview of the Four Layers

| Layer | Name | Purpose |
|-------|------|---------|
| 1 | Client Context | Parse the client message to identify stated problem, urgency, and willingness to adjust |
| 2 | Campaign Trend Analysis | Analyze historical monthly data to establish baselines, identify trends, quantify gaps, classify root cause |
| 3 | Source-Level Diagnosis | Drill into source settings to identify where opportunity exists and where performance is lost |
| 4 | Market Pricing Calibration | Cross-reference effective bids against RightPricing market data to calibrate recommendations |

**Critical principle:** Source-level performance data alone is insufficient for bid recommendations. Without market pricing context (RightPricing report), it is impossible to determine whether the issue is bid amount, bid rate, targeting, or filter settings. A source appearing to need a higher bid may already be bidding above first position.

## Data Requirements

Before beginning, collect:

**Required:** Monthly Campaign CSVs (min 3 months, 6 ideal) containing Searches, Impressions, Clicks, Leads, Calls, CTR, Cost, Avg CPC, Avg Bid, Avg Pos, Win Rate, Sold, Sold CVR, Sold CPA, Sold Rev. Also: Client message or stated problem/goals.

**Strongly Recommended:** Source Settings Export (all traffic sources with Status, Bid Multiplier, Opportunities, Target Rate, Bids, Bid Rate, Impressions, Clicks, Leads, Win Rate, Sold, CPC, Profit, CM). RightPricing Report (trailing 7-day market pricing by source type and ad wall position).

**Optional:** Ad Group Settings, Historical Bid Changes, Competitive Context.

If the user has not provided all required inputs, ask for them before proceeding. If strongly recommended inputs are missing, note the limitation and proceed with what is available, flagging where recommendations would be more precise with additional data.

## Workflow

### Layer 1: Client Context Parsing

1. Extract the stated problem from the client message (low volume, high CPA, low conversion, declining ROAS)
2. Identify willingness to adjust — what levers is the client open to pulling (bids, budget, targeting, sources, schedules)
3. Note implicit constraints — what was NOT mentioned limits recommendation scope

### Layer 2: Campaign Trend Analysis

1. Build a monthly trend table from each CSV: Searches, Impressions, Impression Share, Clicks, CTR, Leads, Cost, Avg CPC, Avg Bid, Win Rate, Sold, Sold CPA
2. Handle partial months by projecting: `Projected Value = (Actual / Days Elapsed) × Total Days in Month` — label as estimates
3. Calculate baselines (averages across complete months) and quantify gaps vs. current/projected month
4. Classify root cause using the diagnostic matrix (see `references/diagnostic-matrices.md`)
5. For accounts under 90 days, shift to leading indicators (Contact Rate, Quote Rate) instead of CPA — see maturity phase analysis in `references/analysis-framework.md`

### Layer 3: Source-Level Diagnosis

1. Filter to Active sources with Opportunities > 0
2. Calculate Effective Bids: `Effective Bid = Base Bid × (Multiplier / 100)`
3. Rank sources by Opportunity volume descending — focus on highest-volume sources first
4. Classify each source's problem type using bid rate / win rate diagnostics (see `references/diagnostic-matrices.md`)
5. Evaluate off-hours bidding opportunity for budget-constrained clients (50–65% bid modifier on non-peak windows)

### Layer 4: Market Pricing Calibration

1. Map each source's Effective Bid to RightPricing positions (Above 1st, Between 1st–2nd, Between 2nd–3rd, Below 3rd)
2. Cross-reference position with the source problem type to generate the specific recommendation
3. Calculate target multipliers where raising is indicated: `Target Multiplier = (Target RPC / Base Bid) × 100%` — round up 5–10% for bid volatility
4. Flag investigation items where bid changes alone cannot fix the anomaly

### Generating Final Recommendations

1. **Prioritize by impact** — highest opportunity volume sources with clear fixes first
2. **Categorize** into three groups: Multiplier Adjustments (with current, recommended, effective bid, and target position), Investigation Items (anomalies requiring follow-up), Hold Steady (well-performing sources with rationale for no change)
3. **Respect client constraints** from Layer 1 — match recommendation aggressiveness to stated willingness
4. **Present with context** — every recommendation includes what to change, why (data-linked), expected directional impact, and caveats to monitor

## Disposition Data Preparation

When client CRM data is involved, apply the Three-Column Rule: add `contacted`, `quoted`, `bound` columns with binary coding (1/0). Ensure Phone Number is the unique key in clean 10-digit format (strip all dashes, parentheses, country codes). This is the #1 cause of match-back failures.

## Output Format

Present the analysis as a structured report following the layer sequence. Include:
- A trend summary table
- Root cause classification with supporting data
- Source-by-source diagnostic table showing opportunity volume, bid rate, win rate, effective bid, market position, and recommendation
- Prioritized recommendation list grouped by category
- Monitoring plan for post-implementation

## Additional Resources

### Reference Files

For detailed procedures, decision tables, and advanced tactics, consult:

- **`references/analysis-framework.md`** — Full step-by-step procedures for all four layers, disposition data preparation, account maturity analysis, and off-hours optimization
- **`references/diagnostic-matrices.md`** — All diagnostic decision tables: root cause classification, source problem typing, market position mapping, and recommendation crossover matrix
- **`references/formulas-pitfalls-tactics.md`** — Key formulas reference, the seven common pitfalls to avoid, the 2× Base Bid Strategy, and the analysis checklist
