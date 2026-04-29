# AI for Capacity Planning and Traffic Prediction in RAN

**Source:** Nokia Bell Labs, "AI-driven network planning with MIKA" (2024); Ericsson Technology Review, "Machine learning in network planning and optimization" (2023); Heavy Reading, "AI-driven RAN planning and optimization" (2024)
**Supporting:** GSMA Intelligence use case adoption survey (2024) — 74% operator adoption claim; Spirent, "Predictive network capacity management" (2023)
**Quality tier:** [VND] Vendor documentation (Nokia, Ericsson) + [IND] analyst corroboration
**Relevant to:** `02-use-cases/capacity-planning.md`, `01-landscape.md`

---

## Why this use case is underrepresented in the guide

Capacity planning has the highest adoption rate of any RAN AI use case (~74% per GSMA) but generates the least conference buzz — because it doesn't involve a new radio architecture or a novel ML technique. It's a well-understood problem (predict traffic, size capacity ahead of demand) solved with well-understood tools (time-series forecasting, gradient boosting, LSTM models). Vendors have been shipping it quietly in OSS/BSS tooling for years.

This is precisely why it deserves careful treatment in the guide: it's the best evidence that production RAN AI is less exotic than vendor marketing suggests, and that the most deployable AI is often the most boring.

---

## What it is

**Core function:** Predict traffic demand at cell / sector / cluster level across time horizons:
- Short-term (hours): dynamic spectrum and carrier configuration
- Medium-term (days/weeks): maintenance scheduling, temporary capacity additions
- Long-term (months/quarters): capex planning, site acquisition, frequency licensing

**Where the model lives:** Almost always external to the RAN — runs in OSS (Operations Support System) or a dedicated planning tool. This is **AI+RAN**, not AI on RAN or AI in RAN. The model reads KPI exports, generates a forecast or recommendation, and a human (or an automation layer) acts on it.

---

## Vendor implementations

### Nokia MIKA (Machine Intelligence for Kognitiv Architecture)

MIKA is Nokia's AI/ML platform embedded across their OSS products. For capacity planning specifically:
- Ingests PM (Performance Measurement) counters at 15-minute intervals per cell
- Applies ensemble forecasting: seasonal decomposition + gradient boosting + LSTM
- Outputs: congestion risk score per cell per hour over a 90-day horizon
- Integration: runs inside Nokia NetAct or Nokia Network Services Platform; no separate deployment required

**Key operational detail:** MIKA's capacity forecasts are recommendations, not automated actions. A planning engineer reviews the congestion risk map and schedules capacity changes (carrier additions, parameter tuning, new site procurement). The automation level is Stage 2 (AI-assisted, human decides).

**Documented results:** Nokia claims 15% capex efficiency improvement in named deployments (Elisa Finland, Tele2 Sweden). Methodology: compare actual vs. forecasted congestion; sites flagged by MIKA received early intervention vs. reactive expansion.

### Ericsson OSS AI / Network Manager

Ericsson's capacity planning AI is embedded in their Network Manager (formerly ENM) product:
- Cell-level traffic trend analysis using historical PM data
- Anomaly detection to identify cells growing faster than trend (events: stadium opening, new residential development)
- What-if simulation: project impact of network change on surrounding cluster

**Key operational detail:** Ericsson's tool focuses on the medium-term planning cycle — it's an input to the quarterly capex review, not a real-time control loop. Output is a prioritized list of "at-risk" cells for the next 90 days.

### Common pattern across vendors

Both Nokia and Ericsson's implementations share the same architecture:
1. Input: historical PM counter exports (15-min or 1-hour resolution)
2. Model: ensemble of time-series methods (no single dominant approach)
3. Output: congestion risk forecast + ranked intervention list
4. Action: human planner reviews and approves

Neither implementation requires new data sources, new hardware, or new interfaces. They consume what the OSS already has. This is why deployment is in "quarters" (time to train on historical data and integrate into planning workflows) rather than "weeks."

---

## Evidence quality assessment

The 15% capex efficiency figure from Nokia and Ericsson needs scrutiny:
- **Baseline methodology unclear:** Is this comparing AI-assisted planning to previous planning cycles at the same operator? Or to a counterfactual (what would have happened without AI)?
- **Selection bias:** Both case studies use operators with mature OSS data hygiene. The result may not generalize to operators with fragmented counter data.
- **Named operators:** Elisa (Finland) and Tele2 (Sweden) are tier-1 Nordic operators with well-maintained networks — a favorable deployment environment.

**Calibrated claim for the guide:** 8–15% capex efficiency improvement in favorable conditions (clean OSS data, mature planning processes). Operators with fragmented PM data will see lower gains.

---

## Why this is Tier 1 (deploy now)

Four conditions that put this in the highest maturity tier:

1. **No new data required:** runs on existing PM counter exports already in the OSS
2. **No new interfaces:** all vendors embed this in existing OSS products
3. **Human-in-the-loop:** Stage 2 automation — no autonomous action risk
4. **ROI is verified against capex:** operators can compare actual vs. forecasted congestion; the counterfactual is measurable in hindsight

**The catch:** The value is proportional to the quality of PM data and the operator's planning process maturity. An operator that already does rigorous quarterly capex review gains less than one with informal planning.

---

## What the guide should say

This use case is Tier 1 but gets less attention than energy optimization because:
1. It's embedded in vendor OSS — operators often don't realize they already have it
2. The ROI shows up in capex avoidance, which is harder to celebrate than energy savings
3. It doesn't require a new purchase decision — it's a configuration and training exercise

**Practical implication for operators:** Before evaluating any new RAN AI use case, check whether your existing OSS vendor already provides capacity planning AI that you're not using. This is the most common "low-hanging fruit" finding in RAN AI maturity assessments.
