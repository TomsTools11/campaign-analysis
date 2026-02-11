# GOAL Campaign Analysis — Full Framework Reference

## Layer 1: Client Context Parsing — Detailed Procedures

Every analysis begins with the client's message. This step establishes the problem frame and determines what levers the client is willing to pull.

### Step 1: Extract the Stated Problem

Read the client message and identify the specific complaint. Common categories:

- **Low lead volume** — "We're not getting enough leads"
- **High CPA** — "Our cost per acquisition is too high"
- **Low conversion rates** — "Leads aren't converting"
- **Declining ROAS** — "Return on ad spend is dropping"
- **General dissatisfaction** — "Performance feels off"

Example: Perry Agency message — "Hey! We're still getting just a really low volume of leads. How do we fix that. I don't mind raising the bid or budget a little."
- Problem: Low lead volume
- Willingness: Raise bid or budget "a little" (moderate, not aggressive)
- Implicit constraint: "A little" suggests incremental changes, not a 2× increase

### Step 2: Identify Willingness to Adjust

Determine what the client has explicitly said they're open to changing. Common levers:

- Raising bids (multipliers or base bid)
- Increasing budget
- Expanding targeting (new geographies, demographics)
- Adding sources (enabling currently inactive sources)
- Adjusting schedules (day parting, off-hours)

### Step 3: Note Implicit Constraints

What the client did NOT say is equally important. If they only mention bids, do not recommend budget changes without flagging it as an additional option. Respect the scope of their ask.

---

## Layer 2: Campaign Trend Analysis — Detailed Procedures

### Step 1: Build the Monthly Trend Table

Extract these metrics from each monthly CSV and compile into a single trend table:

| Metric | What It Reveals |
|--------|-----------------|
| Searches | Total market demand. Is the market shrinking or stable? |
| Impressions (& Share) | How often the ad is shown. Impression Share = Impressions / Searches. A drop means competitors are outbidding. |
| Clicks | Traffic received. CTR = Clicks / Impressions. Drop in CTR may indicate position or ad quality issues. |
| Leads | Primary volume metric — usually the core complaint. |
| Cost | Total spend. Rising cost with falling leads = efficiency problem. |
| Avg CPC | What the client actually pays per click. Compare to Avg Bid for auction efficiency. |
| Avg Bid | The client's bid level. Has it changed? Is it keeping pace with the market? |
| Win Rate | % of bids winning the auction. THE most diagnostic metric for competitive positioning. |
| Sold / Sold CPA | Downstream conversion and cost-per-acquisition. Indicates whether leads actually convert. |

### Step 2: Handle Partial Months

If the current month is incomplete, project the full month:

```
Projected Monthly Value = (Actual Value / Days Elapsed) × Total Days in Month
```

Apply this to Leads, Searches, Cost, and other volume metrics. Clearly label projected values as estimates. The projection becomes more reliable after 10+ days of data.

### Step 3: Calculate Baselines and Identify the Gap

Compute the average of each metric across all complete months. Then compare the current/projected month to the baseline:

- **Lead Volume Gap**: How many fewer leads vs. average? What % below baseline?
- **Win Rate Gap**: Current win rate vs. historical average. A drop of >3 percentage points is significant.
- **Impression Share Gap**: Current vs. average. Drops of >5pp indicate competitive pressure.
- **Spend Gap**: Is spending lower because of fewer wins, or has budget been reduced?

### Step 4: Root Cause Classification

Use the diagnostic matrix in `diagnostic-matrices.md` to classify the root cause.

### Step 5: Maturity Phase Analysis (Accounts Under 90 Days)

For accounts active for less than 90 days, standard CPA-driven analysis produces unreliable results. Disposition data takes time to mature — leads need to move through quoting and binding before Sold CPA becomes meaningful.

**Shift focus to leading indicators:**

- **Contact Rate** = Leads Contacted / Total Leads Delivered
- **Quote Rate** = Quotes Issued / Leads Contacted

These metrics mature faster than Sold CPA and reveal whether the agency's speed-to-lead and quoting process are functioning.

**Diagnostic Logic for New Accounts:**

| Contact Rate | Quote Rate | Interpretation |
|-------------|------------|----------------|
| >50% | High | Leads are strong. Agency is engaging. CPA will mature favorably. Hold current settings. |
| <50% | High (on contacted) | Issue is speed-to-lead, not lead quality. Coach client on response time. |
| >50% | Low | Leads contacted but not converting to quotes. Possible targeting mismatch. Investigate filters and ad group settings. |
| <50% | Low | Insufficient data. Could be agency process or lead quality. Extend window and revisit at 90 days. |

**Talking point for new accounts:** If Contact Rate is below 50% but Quote Rate on contacted leads is high, the issue is speed-to-lead, not lead quality. Frame it: "The leads you're reaching are converting well — the opportunity is in getting to more of them faster."

---

## Layer 3: Source-Level Diagnosis — Detailed Procedures

### Step 1: Filter to Active Sources with Activity

From the Source Settings export, filter to sources that are both Active AND have Opportunities > 0. Inactive sources and sources with zero opportunities can be ignored for bid optimization. Note them separately if the client asks about enabling new sources.

### Step 2: Calculate Effective Bids

For each active source:

```
Effective Bid = Base Bid (from campaign Avg Bid) × (Multiplier / 100)
```

This is what the client actually bids per click on each source.

### Step 3: Rank Sources by Opportunity Volume

Sort active sources by Opportunities descending. Focus recommendations on top sources by opportunity volume — small multiplier changes on high-volume sources have much larger impact than large changes on low-volume sources.

### Step 4: Classify Each Source's Problem

Use the source diagnostic matrix in `diagnostic-matrices.md`.

### Step 5: Off-Hours Optimization Strategy

For budget-conscious clients, activating off-hours bidding provides a way to capture additional lead volume at a significant discount.

**Strategy:** Recommend activating off-hours windows (e.g., 6 PM – 9 PM local time, or weekends) with a **50–65% bid modifier** applied to those hours. This means the client pays roughly half of their base bid during non-peak periods. **Do not set it to 150% — that would increase the bid, not reduce it.**

**When to Recommend:**
- Client is budget-constrained but wants more volume
- Day parting is currently off or set to business hours only
- Win rate during peak hours is already competitive

**Implementation:** Set the schedule to include off-hours windows and apply a 50–65% bid modifier. Monitor for 7–14 days. If off-hours leads show acceptable contact and quote rates, consider expanding the window or increasing the modifier to 70–75%.

---

## Layer 4: Market Pricing Calibration — Detailed Procedures

### Pulling the RightPricing Report from Periscope

1. Log into Periscope/Sisense and navigate to the RightPricing dashboard
2. Filter for the specific **state and vertical** (e.g., Auto — TX, Home — CO). Do not pull aggregate data across states or verticals
3. Set the date range to exactly **"Last 7 Days"** — this trailing window reflects current market pricing and avoids historical noise
4. Export or screenshot the report showing RPC at positions 1 through 3+ for each source type

**Sanitize the export before presenting to a client:** Delete the 'Inventory' column. Clients often conflate 'Inventory' (available auctions) with 'Traffic' (actual clicks), leading to unrealistic volume expectations. Only share the RPC columns for each position.

### Step 1: Map Effective Bids to Market Positions

For each active source, compare the Effective Bid to the RightPricing data. Determine which ad wall position the bid corresponds to. See position mapping table in `diagnostic-matrices.md`.

### Step 2: Cross-Reference Position with Source Problem Type

This is the diagnostic crossover that generates the actual recommendation. See the recommendation crossover matrix in `diagnostic-matrices.md`.

### Step 3: Calculate Target Multipliers (When Raising)

```
Target Multiplier = (Target RPC / Base Bid) × 100%
```

Choose the Target RPC based on client willingness:
- "A little" = target 2nd position
- "Willing to be aggressive" = target 1st position

Always round up slightly (5–10%) to account for bid volatility and ensure consistent positioning.

---

## Disposition Data Preparation — Full Workflow

### Step 1: Export Raw Leads from Client CRM

Request or pull the raw lead export from the client's system (Ricochet, Lead Manager, or equivalent). This should contain every lead the client received during the analysis period.

### Step 2: Apply the Three-Column Rule

Add three new columns: `contacted`, `quoted`, and `bound`. These represent the disposition funnel that GOAL uses for match-back analysis.

### Step 3: Use Binary Coding

Populate each column using binary indicators: **1 for yes, 0 for no.** Do not use text statuses. The GOAL system ingests binary values for match-back processing.

**CRM-Specific Mapping (Ricochet / Lead Manager):**
- Map 'Sold', 'Bound', and 'Customer' → 1 in `bound` column
- Map 'Quoted' and 'Sent to Carrier' → 1 in `quoted` column
- Strip everything except Phone Number, contacted, quoted, and bound before uploading

### Step 4: Set Phone Number as the Unique Key

Phone Number must be present as the unique identifier for every row. This is the field used to map client disposition data back to GOAL prospects. Without a clean phone number match, the match-back will fail silently.

**Critical:** Always format phone numbers as clean 10-digit strings (e.g., `5551234567`). Strip all parentheses, dashes, spaces, and country code prefixes. A single formatting mismatch causes an entire lead's disposition to be silently dropped.

---

## Generating Final Recommendations — Detailed Procedures

### Step 1: Prioritize by Impact

Rank recommendations by potential volume impact. The biggest opportunity pools with clear fixes come first. A small change on a source with 2,000 opportunities is more impactful than a large change on a source with 40.

### Step 2: Categorize Recommendations

Group into three categories:

**Multiplier Adjustments:** Specific percentage changes with rationale. Include the current multiplier, recommended multiplier, effective bid at new multiplier, and the market position it targets.

**Investigation Items:** Sources where data shows an anomaly that multiplier changes cannot resolve. Flag for follow-up with the client or GOAL product team.

**Hold Steady:** Sources performing well that should not be changed. Explain why — clients need to understand that leaving a well-performing source alone is a deliberate decision, not oversight.

### Step 3: Respect Client Constraints

Map recommendations back to the client's stated willingness from Layer 1. If they said "a little," do not recommend 3× increases. If budget is a concern, prioritize efficiency (investigation items) over volume (bid increases).

### Step 4: Present with Context

Every recommendation must include:
- **What** to change
- **Why** (linking back to the data)
- **Expected impact** (directional, not exact)
- **Caveats** or things to monitor after the change
