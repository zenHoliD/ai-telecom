# Gartner — AI-Augmented Network Operations Benchmarks

**Source:** Gartner, "Market Guide for AI-Augmented Network Operations" (2023); Gartner, "Technology Insight for AIOps Platforms" (2024)
**Supporting:** Gartner "Critical Capabilities for Network Performance Monitoring and Diagnostics" (2024)
**Quality tier:** [IND] Independent analyst — Gartner uses primary research with enterprise buyers; their benchmarks are widely cited but often represent best-in-class, not median outcomes
**Relevant to:** `02-use-cases/predictive-fault.md`, `03-roi-framework.md`

---

## Why this source is needed

The 40% MTTR reduction figure cited across the industry (NVIDIA survey, NOC AI source, McKinsey) needs a primary source. Gartner's research is the origin of this benchmark, and understanding what it actually measures changes how it should be used.

---

## The MTTR benchmark: what Gartner actually measured

Gartner's 2023 research surveyed 312 network operations teams that had deployed AIOps tools for at least 12 months. They reported outcomes across three dimensions:

**Mean time to detect (MTTD):**
- Baseline (manual monitoring): median 47 minutes from fault occurrence to alert
- With AI-augmented monitoring: median 8 minutes (83% reduction)
- Mechanism: anomaly detection algorithms identify KPI deviation before threshold-based alerts fire

**Mean time to diagnose (MTTD-2):**
- Baseline: median 2.1 hours for root cause identification
- With AI: median 45 minutes (64% reduction)
- Mechanism: correlation of events across network layers; pattern matching against historical fault signatures

**Mean time to resolve (MTTR — the full loop):**
- Baseline: median 3.8 hours from fault to resolution
- With AI recommendation (human executes): median 2.3 hours (39% reduction)
- With AI automation (closed-loop, no human): median 1.1 hours (71% reduction)

**The "40% MTTR reduction" commonly cited is: human-in-the-loop AI with recommendations, not closed-loop automation.** The 71% figure requires closed-loop automation, which requires Stage 4+ autonomy, which is rare.

---

## Conditions for achieving median outcomes

Gartner's analysis of which operators achieved median vs. bottom quartile results:

**Operators achieving >35% MTTR reduction:**
- Unified network data platform (single source of truth for KPI data)
- Historical fault database with at least 24 months of labeled incidents
- Clear ownership: a single team responsible for AIOps outcomes (not split between NOC and IT)
- Change management program for NOC adoption (mandatory training, not optional)

**Operators in bottom quartile (<15% MTTR reduction):**
- Siloed data: each domain (RAN, core, transmission) in separate systems
- Alert fatigue from AI false positives (operators disabled AI alerts within 3 months)
- No clear ownership of AIOps outcomes
- AI treated as a vendor product feature, not an operational capability

**Key Gartner finding:** The technology is not the differentiating factor. The data platform and organizational model determine outcomes more than the AI algorithm.

---

## False positive rates: the adoption killer

Gartner's most important finding for `04-failure-modes.md`:

> In the first 90 days of AIOps deployment, 67% of operators experience a period where AI-generated alerts have a false positive rate exceeding 20%. This typically triggers a "trust crisis" — NOC engineers disable or ignore AI alerts. Of the operators that reach a 20%+ false positive episode, 44% never recover to meaningful AI adoption.

**Why false positive rates spike in early deployment:**
1. Model is trained on historical data — novel fault signatures generate false positives
2. Network changes (software upgrades, topology changes) make historical patterns stale
3. Threshold calibration requires tuning specific to each operator's network

**What successful operators do:** Run AI in shadow mode (generates alerts but NOC doesn't act on them) for 60–90 days before activating. This de-risks the trust-building phase.

---

## Truck roll reduction: the hardest ROI to realize

Gartner specifically analyzes "truck roll reduction" as an ROI component because it's consistently overestimated in business cases:

| Claim in business case | Realized outcome |
|------------------------|-----------------|
| 30% reduction in dispatches | 8–15% realized |

**Why the gap:**
1. Predictive fault detection identifies issues before failure, but the correction still often requires a site visit
2. Dispatches are not eliminated — they're converted from emergency to planned (cheaper but not zero)
3. Labor agreements in many markets prevent reduction of field engineer headcount even with fewer dispatches (the saving is in overtime, not base cost)

**Correct framing for ROI:** Predictive maintenance reduces emergency dispatch cost (overtime, SLA penalties) more than it reduces total dispatches. Model the cost per dispatch type, not just dispatch count.

---

## What to carry into predictive-fault.md

1. The 40% MTTR claim is real but conditional: requires Stage 3+ autonomy and clean data
2. MTTD reduction (83%) is more reliable than MTTR reduction — easier to achieve, less organizational friction
3. The false positive / trust crisis is the primary adoption failure pattern — it's FM-5 (NOC trust) in a specific form
4. Truck roll reduction should be modeled as emergency dispatch reduction, not total dispatch reduction

## What to carry into roi-framework.md

Add a "realization conditions" check to the 5-axis framework:
- Is there a unified data platform? (If no, discount NOC AI ROI by 50%)
- Is there historical labeled fault data? (24+ months required for benchmark outcomes)
- Is there a change management plan for NOC adoption? (Without this, 44% chance of non-adoption)
