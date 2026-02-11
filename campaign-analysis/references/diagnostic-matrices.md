# GOAL Campaign Analysis — Diagnostic Matrices & Decision Tables

## Layer 2: Root Cause Classification Matrix

Use campaign-level trend data to determine the primary root cause category:

| Searches | Win Rate | Impression Share | Diagnosis |
|----------|----------|-----------------|-----------|
| Stable/Up | Down | Down | **Competitive Positioning** — Being outbid by competitors |
| Down | Stable | Stable | **Market Demand Decline** — Fewer searches in market |
| Stable/Up | Stable | Down | **Budget Constraint** — Running out of daily budget before market clears |
| Down | Down | Down | **Market + Competitive** — Both demand decline and competitive pressure |

### Interpreting the Matrix

- **Competitive Positioning** is the most common root cause. Win Rate and Impression Share dropping while Searches remain stable means competitors are outbidding the client.
- **Market Demand Decline** is seasonal or macro-driven. Fewer people searching means less inventory — bids may be fine but volume is naturally lower.
- **Budget Constraint** shows up when Impression Share drops but Win Rate holds. The client wins when they bid, but they're running out of budget and missing later auctions.
- **Market + Competitive** is the hardest to fix — both the market is smaller and the client is losing more of what remains.

### Perry Agency Example

- Searches: ~1,635 projected (below 1,929 avg but not dramatic)
- Win Rate: 4.45% vs. 12.3% avg (crashed 64%)
- Impression Share: 10.3% vs. 25.5% avg (crashed 60%)
- **Diagnosis: Competitive Positioning** — Perry was being significantly outbid. The market was slightly softer but the real problem was competitive pressure.

---

## Layer 2: Account Maturity Diagnostic (Accounts Under 90 Days)

| Contact Rate | Quote Rate | Interpretation |
|-------------|------------|----------------|
| >50% | High | Leads are strong. Agency is engaging. CPA will mature favorably. Hold current settings. |
| <50% | High (on contacted) | Issue is speed-to-lead, not lead quality. Agency not reaching leads fast enough. Coach client on response time. |
| >50% | Low | Leads contacted but not converting to quotes. Possible targeting mismatch or lead quality issue. Investigate filters and ad group settings. |
| <50% | Low | Insufficient data to diagnose. Could be agency process or lead quality. Extend analysis window and revisit at 90 days. |

---

## Layer 3: Source Problem Classification Matrix

Use source-level bid rate and win rate to classify the problem type:

| Bid Rate | Win Rate | Leads | Problem Type |
|----------|----------|-------|-------------|
| Low (<15%) | Any | Low | **Bid Rate Problem** — Not entering enough auctions. Check targeting filters, ad group settings, or technical issues upstream of the bid. |
| Normal (≥15%) | Low (<10%) | Low | **Win Rate Problem** — Entering auctions but losing. Likely underbidding for market position. |
| Normal (≥15%) | High (>30%) | Good | **Performing Well** — May be overpaying. Check if bid can be reduced without losing position. |
| Zero (0%) | N/A | Zero | **Not Bidding** — May have filter/targeting issues or source may be newly enabled. Investigate before adjusting multiplier. |

### Perry Agency Source Diagnosis Example

| Source | Opps | Bid Rate | Win Rate | Classification |
|--------|------|----------|----------|----------------|
| Tier 1 | 2,176 | 24.58% | 2.06% | Win Rate Problem (bidding plenty, losing auctions) |
| Tier 3 | 1,099 | 2.09% | 30.43% | Bid Rate Problem (wins when it bids, but barely bidding) |
| Internal SEO | 96 | 13.54% | 84.62% | Performing Well |
| Tier 2 | 56 | 16.07% | 0% | Win Rate Problem |
| Home SearchB | 40 | 0% | N/A | Not Bidding |

---

## Layer 4: Market Position Mapping Table

Compare the client's Effective Bid to the RightPricing RPC data for each source:

| Position | What It Means | Typical Click Share |
|----------|---------------|---------------------|
| Above 1st RPC | Client bids above the highest-paying advertiser | 78–84% of available clicks |
| Between 1st and 2nd RPC | Competitive but not dominant | 40–60% of clicks |
| Between 2nd and 3rd RPC | Mid-pack, moderate volume | 15–30% of clicks |
| Below 3rd RPC | Bottom of ad wall, scraping leftovers | <10% of clicks |

---

## Layer 4: Recommendation Crossover Matrix

Cross-reference the source problem type (from Layer 3) with the market position (from Layer 4) to generate the specific recommendation:

| Source Problem | Market Position | Recommendation |
|----------------|-----------------|----------------|
| Win Rate Problem | Below 1st RPC | **RAISE MULTIPLIER** — Clear underbid. Calculate target multiplier to reach first position. |
| Win Rate Problem | Above 1st RPC | **INVESTIGATE** — Bid is already above market. Problem is NOT the multiplier. Check targeting, filters, or ad group settings. Consider contacting GOAL product team. |
| Bid Rate Problem | Any position | **DO NOT RAISE MULTIPLIER** — Source barely enters auctions regardless of bid level. Problem is upstream: targeting filters, ad group settings, or technical issue. Raising multiplier only increases spend on the tiny fraction of auctions entered. |
| Performing Well | Above 1st RPC | **CONSIDER REDUCING** — Client may be overpaying. Test lowering multiplier 5–10% and monitor win rate for impact. |
| Not Bidding | Any position | **INVESTIGATE FIRST** — Before adjusting multiplier, determine why zero bids are being placed. Check filters, targeting, and source status. |

### Perry Agency Calibration Example

| Source | Effective Bid | 1st RPC | Position | Recommendation |
|--------|--------------|---------|----------|----------------|
| Tier 2 | $11.48 | $16.62 | Below 1st | Raise to 140% ($16.74 effective) |
| Home SearchB | $23.80 | $28.75 | Below 1st | Raise to 245% ($29.30 effective) |
| Tier 3 | $12.44 | $8.19 | Above 1st | DO NOT RAISE. Bid rate problem (2.09%). Investigate targeting. |
| Tier 1 | $13.87 | $9.84 | Above 1st | Modest raise to 125% + investigate. Something besides bid causing losses. |

**Critical lesson from Perry Agency:** Initial recommendations based only on source performance data suggested raising Tier 3 from 104% to 115–120%. But RightPricing showed Tier 3's effective bid ($12.44) was already 52% ABOVE first position pricing ($8.19). The real problem was bid rate (2.09%), not bid amount. Raising the multiplier would have wasted money.
