## GOAL Campaign Analysis Skill

This repository defines the **GOAL Platform Campaign Analysis** skill: a systematic, multi-layer framework for diagnosing GOAL client campaign performance and generating calibrated bid and multiplier recommendations.

The skill is designed to be invoked when an AM wants to “analyze a GOAL client campaign,” “diagnose campaign performance,” “optimize bid multipliers,” or similar, and when campaign CSVs and source/RightPricing exports are available.

## What This Skill Does

The skill walks through a four-layer analysis:

### 1) Client Context

Parses the client’s message to identify:

* The stated problem (e.g., low lead volume, high CPA, declining ROAS)
* Urgency
* Which levers they are willing to pull (bids, budget, targeting, sources, schedules)

It also infers implicit constraints (for example, “a little” vs “aggressive” changes).

### 2) Campaign Trend Analysis

Uses monthly campaign CSVs (3+ months, 6 ideal) to:

* Build a trend table for key metrics:
    * Searches, Impressions, Impression Share, Clicks, CTR, Leads, Cost, Avg CPC, Avg Bid, Win Rate, Sold, Sold CPA
* Project partial months using:

    `Projected Value = (Actual / Days Elapsed) × Total Days in Month`

* Establish baselines and quantify gaps vs. current month
* Classify the root cause using the **Root Cause Classification Matrix**:
    * Competitive positioning
    * Market demand decline
    * Budget constraint
    * Mixed
* For accounts under 90 days, pivot from Sold CPA to **Contact Rate** and **Quote Rate** using the **Account Maturity Diagnostic**

### 3) Source-Level Diagnosis

Drills into the Source Settings export to:

* Filter to active sources with Opportunities > 0
* Compute Effective Bid for each source:

    `Effective Bid = Base Bid (Avg Bid from campaign CSV) × (Multiplier / 100)`

* Rank sources by Opportunities to focus on high-impact levers
* Classify each source using the **Source Problem Classification Matrix**:
    * Bid Rate Problem
    * Win Rate Problem
    * Performing Well
    * Not Bidding
* Evaluate off-hours optimization for budget-constrained clients via day parting (typically 50–65% bid modifiers on non-peak hours)

### 4) Market Pricing Calibration (RightPricing)

Aligns source-level recommendations with live market pricing:

* Pulls the trailing 7-day RightPricing report (Periscope/Sisense), filtered to the correct state and vertical
* Maps each source’s Effective Bid to a market position:
    * Above 1st
    * Between 1st–2nd
    * Between 2nd–3rd
    * Below 3rd
* Uses the **Recommendation Crossover Matrix** to decide whether to:
    * Raise multipliers
    * Hold steady
    * Reduce multipliers
    * Treat as investigation items (when bid is not the true problem)
* Calculates Target Multipliers when raising is appropriate:

    `Target Multiplier = (Target RPC / Base Bid) × 100%`

    Then rounded up 5–10% for auction volatility.

## Data Requirements

To run a complete analysis, the skill expects:

### Required

* Monthly campaign CSVs (min 3 months, 6 ideal) with:
    * Searches, Impressions, Clicks, Leads, Calls, CTR
    * Cost, Avg CPC, Avg Bid, Avg Position
    * Win Rate, Sold, Sold CVR, Sold CPA, Sold Revenue
* The client’s message or stated problem/goals

### Strongly Recommended

* Source Settings Export including:
    * Status, Bid Multiplier, Opportunities
    * Target Rate, Bids, Bid Rate, Impressions, Clicks, Leads
    * Win Rate, Sold, CPC, Profit, Contribution Margin
* RightPricing Report (trailing 7-day market RPC by source type and ad wall position)

### Optional

* Ad Group settings
* Historical bid changes
* Competitive context
* Client CRM disposition export (for contact/quote/bound analysis)

If some required inputs are missing, the skill should request them before proceeding. If strongly recommended inputs are absent, it proceeds but flags that recommendations are less precise, especially around market calibration.

## Core Formulas

Key calculations used throughout the framework include:

* Effective Bid  
    `Base Bid × (Source Multiplier / 100)`
* Target Multiplier  
    `(Target RPC from RightPricing / Base Bid) × 100%`
* Projected Monthly Leads (and other volumes)  
    `(Leads to Date / Days Elapsed) × Days in Month`
* Impression Share  
    `Impressions / Searches`
* Bid Rate  
    `Bids / Opportunities`
* Win Rate  
    `Wins (Impressions) / Bids`
* Contact Rate  
    `Leads Contacted / Total Leads Delivered`
* Quote Rate  
    `Quotes Issued / Leads Contacted`

## Diagnostic Matrices

The references in `references/` codify the decision logic:

* Root Cause Classification Matrix (Layer 2)  
    Uses trends in Searches, Win Rate, and Impression Share to classify:
    * Competitive Positioning
    * Market Demand Decline
    * Budget Constraint
    * Market + Competitive (mixed)

* Account Maturity Diagnostic (Under 90 Days)  
    Interprets Contact Rate and Quote Rate to distinguish lead quality issues from speed-to-lead or process issues.

* Source Problem Classification Matrix (Layer 3)  
    Uses Bid Rate, Win Rate, and Leads to label each source as:
    * Bid Rate Problem
    * Win Rate Problem
    * Performing Well
    * Not Bidding

* Market Position Mapping Table (Layer 4)  
    Maps each Effective Bid to a RightPricing position (Above 1st, 1st–2nd, 2nd–3rd, Below 3rd).

* Recommendation Crossover Matrix  
    Crosses the source problem type with the market position to generate the concrete recommendation (e.g., raise multiplier, investigate targeting, consider reducing).

## Advanced Tactics

The framework also includes several advanced strategies:

* 2× Base Bid Strategy  
    For aggressive-growth clients with budget headroom:
    * Set Base Bid ≈ 2× Target RPC
    * Use day parting (e.g., 50% modifiers off-peak) to control blended effective bids while winning more competitive auctions  
    Strictly reserved for clients explicitly open to aggressive spend; not appropriate when they only want to “raise a little.”

* Off-Hours Optimization  
    For budget-constrained clients:
    * Activate off-hours windows (evenings/weekends) at 50–65% bid modifiers
    * Monitor for 7–14 days and expand if contact/quote rates are healthy

* Disposition Data Preparation (Three-Column Rule)  
    When CRM data is available:
    * Add `contacted`, `quoted`, `bound` columns with 1/0 coding
    * Use Phone Number as the unique key, formatted as a clean 10-digit string (no spaces, parentheses, dashes, or country codes)
    * Fix phone formatting first when match-back or Sold CPA looks broken

## Seven Pitfalls This Skill Avoids

The framework is explicitly designed to prevent common analysis mistakes, including:

1. Raising multipliers without RightPricing data (bidding above first position by accident).
2. Treating all sources equally instead of weighting by opportunity volume.
3. Ignoring Bid Rate and trying to fix upstream problems with bid changes.
4. Over-relying on campaign-level aggregates rather than source-level metrics.
5. Blindly following the client’s framing (“just raise bids”) instead of the data.
6. Judging health by blended CPA without isolating Paid Search vs. Premium Referral.
7. Failing disposition match-back due to phone number format traps.

## Output: What the Skill Returns

When given the appropriate inputs, the skill should produce a structured report that includes:

* A campaign trend summary table with baselines and current gaps
* A clear root cause classification with supporting data
* A source-by-source diagnostic table showing:
    * Opportunities, Bid Rate, Win Rate
    * Effective Bid and mapped market position
    * Problem type and recommendation
* A prioritized recommendation list, grouped into:
    * Multiplier Adjustments (with current vs. recommended settings and target positions)
    * Investigation Items (where multiplier changes cannot solve the issue)
    * Hold Steady (high-performing sources that should not be changed)
* A short monitoring plan outlining what to watch post-change (lead volume, win rate, impression share, CPA, contact/quote/bound rates)

## Repository Structure

* `SKILL.md` – Top-level description of the GOAL Campaign Analysis skill and its four-layer workflow.
* `references/analysis-framework.md` – Detailed procedures for each layer, including client context parsing, trend analysis, source diagnosis, market calibration, and recommendation presentation.
* `references/diagnostic-matrices.md` – All diagnostic matrices and decision tables used for classification and recommendation logic.
* `references/formulas-pitfalls-tactics.md` – Formula reference, seven common pitfalls, and advanced tactics like 2× Base Bid and off-hours optimization.

