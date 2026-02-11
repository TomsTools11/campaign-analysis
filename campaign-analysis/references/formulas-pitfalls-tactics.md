# GOAL Campaign Analysis — Formulas, Pitfalls & Advanced Tactics

## Key Formulas Reference

| Formula | Calculation |
|---------|-------------|
| Effective Bid | `Base Bid (Avg Bid from campaign CSV) × (Source Multiplier / 100)` |
| Target Multiplier | `(Target RPC from RightPricing / Base Bid) × 100%` |
| Projected Monthly Leads | `(Leads to Date / Days Elapsed) × Days in Month` |
| Impression Share | `Impressions / Searches (from campaign CSV)` |
| Bid Rate | `Bids / Opportunities (from source settings)` |
| Win Rate | `Wins (Impressions) / Bids (from source settings)` |
| Volume Impact Potential | `Opportunities × (Target Bid Rate − Current Bid Rate) × Expected Win Rate` |
| Contact Rate | `Leads Contacted / Total Leads Delivered (from client disposition data)` |
| Quote Rate | `Quotes Issued / Leads Contacted (from client disposition data)` |

---

## Seven Common Pitfalls to Avoid

### 1. Recommending Bid Increases Without Market Pricing Data

This was the single biggest lesson from the Perry Agency analysis. Initial recommendations based only on source performance data suggested raising Tier 3 from 104% to 115–120%. But the RightPricing data showed Tier 3's effective bid ($12.44) was already 52% ABOVE first position pricing ($8.19). The real problem was bid rate (2.09%), not bid amount. Raising the multiplier would have wasted money.

**Rule:** Never recommend a multiplier increase without first checking the source's effective bid against RightPricing market data. If RightPricing data is unavailable, explicitly caveat the recommendation as unvalidated.

### 2. Treating All Sources Equally

A 10% multiplier increase on Tier 1 (2,176 opportunities) has vastly more potential impact than a 50% increase on Home SearchB (40 opportunities). Always weight recommendations by opportunity volume. Present the highest-volume fixes first.

### 3. Ignoring the Bid Rate Metric

Bid rate reveals what percentage of available opportunities the source actually enters auctions for. A low bid rate means the system filters out most opportunities BEFORE the bid amount even matters. Raising the multiplier for a low-bid-rate source only increases what is paid on the tiny fraction of auctions entered — it does not increase the number of auctions entered.

**Rule:** If bid rate is below 15%, the problem is upstream of the bid. Investigate targeting, filters, and ad group settings before touching the multiplier.

### 4. Confusing Campaign-Level Metrics with Source-Level Metrics

Campaign-level Win Rate, Avg Bid, and Impression Share are aggregates across ALL sources. They are useful for trend analysis but misleading for bid optimization. A campaign with 12% win rate may have one source at 85% and another at 2%. Source-level data is required for actionable recommendations.

### 5. Over-Rotating on the Client's Framing

The client said "raise the bid." But the data may show the bid is not the problem. The account manager's job is to use data to identify the real lever, not just execute what the client assumes is the fix. Present the data-driven recommendation alongside the client's suggestion and explain why the data points a different direction when it does.

### 6. Judging Account Health by Blended CPA Alone

A high blended CPA often masks strong performance on individual sources. This is especially common with "Premium Referral" sources, which tend to carry higher CPAs that skew overall account metrics. Meanwhile, "Paid Search" sources may be performing well within target.

Before recommending budget cuts or major bid reductions based on a high blended CPA:

- Isolate Paid Search performance to show the client where they are winning
- Identify which specific sources drive the CPA up (usually 1–2 Premium Referral sources)
- Recommend targeted optimization on underperforming sources rather than across-the-board changes

### 7. The "Format Trap" in Disposition Data

If lead matching fails during the disposition upload, 90% of the time the root cause is phone number formatting. Client CRM exports frequently contain parentheses, dashes, spaces, or country code prefixes (e.g., "(555) 123-4567" or "+1-555-123-4567") that do not match the raw 10-digit format stored in GOAL (e.g., "5551234567").

Before uploading any disposition file, ensure the client export strips all special characters from the Phone Number column to produce a clean 10-digit string. A single formatting mismatch can cause an entire lead's disposition to be silently dropped, making CPA data appear worse than reality.

**When Sold CPA suddenly spikes or match-back rates drop, check phone number formatting first.** Run a quick comparison: do the phone numbers in the client export match the format in the GOAL lead export? If not, reformat and re-upload before drawing conclusions about campaign performance.

---

## Advanced Tactic: The 2× Base Bid Strategy

For clients who are willing to be aggressive on growth and have the budget to support it, the 2× Base Bid Strategy uses bid signaling to win competitive auctions while controlling actual spend through day parting.

### How It Works

1. Set the **Base Bid at 2× the Target RPC** to signal high intent to the auction algorithm
2. Apply **Day Parting modifiers to bid it down by 50%** during non-peak hours
3. Net effect: the client captures premium traffic during peak windows at full bid, while the off-peak discount keeps the blended CPA in check

### When to Recommend

- Client explicitly says they want to grow aggressively and have budget headroom
- Win rate is low across the board despite bids being near 1st position RPC
- Market is highly competitive and standard bidding is not cutting through
- **Never recommend if the client said "a little"** — confirm budget tolerance first

### Implementation

1. Identify the Target RPC from the RightPricing report (1st position)
2. Set Base Bid to 2× Target RPC
3. Configure Day Parting: apply a 50% bid modifier to all off-peak hours (e.g., 6 PM – 8 AM and weekends)
4. Monitor daily for 7 days. The blended effective bid should land near 1.2–1.5× Target RPC depending on traffic distribution

### Risk Warning

This strategy should only be recommended to clients with clear budget tolerance and growth intent. Always confirm budget headroom before recommending 2× bids. Monitor spend daily for the first week to prevent budget blowouts.

---

## Off-Hours Optimization Strategy

For budget-constrained clients who want more volume without blowing budgets on unmatched traffic.

### Strategy

Activate off-hours windows (e.g., 6 PM – 9 PM local time, or weekends) with a **50–65% bid modifier**. This discounts the base bid during non-peak periods, capturing leads other advertisers miss.

**Important:** Set the bid modifier to 50–65% (meaning the client pays roughly half of their base bid). Do NOT set it to 150% — that increases the bid, not reduces it.

### When to Recommend

- Client is budget-constrained but wants more volume
- Day parting is currently off or set to business hours only
- Win rate during peak hours is already competitive — off-hours adds incremental volume without cannibalizing existing performance

### Implementation

Set the schedule to include off-hours windows and apply a 50–65% bid modifier. Monitor for 7–14 days. If off-hours leads show acceptable contact rates and quote rates, consider expanding the window or increasing the modifier to 70–75%.

---

## Analysis Checklist

Use this checklist to ensure every analysis follows the complete framework:

### Layer 1: Client Context
- [ ] Extracted the stated problem from client message
- [ ] Identified willingness to adjust (bid, budget, targeting)
- [ ] Noted implicit constraints and scope of ask

### Layer 2: Campaign Trends
- [ ] Built monthly trend table with all key metrics
- [ ] Projected partial month data (if applicable)
- [ ] Calculated baselines (averages across complete months)
- [ ] Quantified the gap (leads, win rate, impression share)
- [ ] Classified root cause (competitive, demand, budget, mixed)
- [ ] Assessed account maturity (≤90 days: use leading indicators instead of CPA)

### Data Preparation
- [ ] Obtained client disposition data (or requested export)
- [ ] Applied Three-Column Rule (contacted, quoted, bound)
- [ ] Used binary coding (1/0) for disposition columns
- [ ] Verified phone number is unique key for match-back
- [ ] Applied CRM-specific mapping rules (e.g., Ricochet Lead Status)
- [ ] Stripped phone number formatting (no dashes, parentheses, or country codes)

### Layer 3: Source Diagnosis
- [ ] Filtered to active sources with opportunities
- [ ] Calculated effective bids (Base Bid × Multiplier)
- [ ] Ranked sources by opportunity volume
- [ ] Classified each source's problem type (bid rate, win rate, performing, not bidding)
- [ ] Evaluated off-hours bidding opportunity for budget-constrained clients

### Layer 4: Market Calibration
- [ ] Pulled Trailing 7-Day RightPricing report from Periscope
- [ ] Filtered for correct state and vertical
- [ ] Sanitized export (removed Inventory column before client presentation)
- [ ] Mapped effective bids to RightPricing positions
- [ ] Cross-referenced position with source problem type
- [ ] Calculated target multipliers where applicable
- [ ] Identified investigation items (anomalies bid changes cannot fix)

### Final Recommendations
- [ ] Prioritized by impact (highest opportunity volume first)
- [ ] Categorized into multiplier changes, investigations, and hold-steady
- [ ] Respected client constraints from Layer 1
- [ ] Included context and rationale for each recommendation
- [ ] Evaluated 2× Base Bid Strategy for aggressive-growth clients
