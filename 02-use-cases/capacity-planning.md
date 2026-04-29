# Use Case: AI-Assisted Capacity Planning and Traffic Prediction

*Last updated: April 2026*
*Maturity tier: Tier 1 — highest adoption rate of any RAN AI use case*

---

## What it is

ML-based traffic forecasting that predicts congestion before it happens, allowing operators to intervene with capacity changes (carrier additions, parameter tuning, site procurement) on a planned schedule rather than reactively.

**Deployment model:** AI+RAN — model runs in OSS or a dedicated planning tool, reads PM counter exports, outputs recommendations to a human planner. Not embedded in baseband; does not close the loop autonomously.

---

## Why it's Tier 1

- ~74% operator adoption (GSMA Intelligence 2024) — highest of any RAN AI use case
- Runs on existing OSS data; no new interfaces or hardware required
- Human-in-the-loop (Stage 2 automation) — no autonomous action risk
- ROI is measurable against capex avoidance in hindsight

**The paradox:** This is the most deployed RAN AI use case and gets the least conference attention. It's not a new architecture — it's a better planning spreadsheet. That's why it works.

---

## How it works

1. **Data ingestion:** PM counter exports from OSS (15-min or 1-hour resolution per cell)
2. **Forecasting:** Ensemble of time-series methods — seasonal decomposition, gradient boosting, LSTM
3. **Output:** Congestion risk score per cell per hour over a 60–90 day horizon; ranked "at-risk" cell list
4. **Action:** Planning engineer reviews the map, schedules capacity changes in the quarterly capex cycle

**Automation level:** Stage 2 — AI recommends, human decides. No vendor currently offers or recommends fully automated capacity-change execution.

---

## Vendor implementations

| Vendor | Product | Key feature | Automation level |
|--------|---------|-------------|-----------------|
| Nokia | MIKA (within NetAct / NSP) | Ensemble forecasting, what-if simulation | Stage 2 |
| Ericsson | Network Manager AI | Trend analysis, anomaly detection, at-risk cell ranking | Stage 2 |

Both implementations share the same architecture: read PM exports, generate forecast, present to a human planner. Neither requires a separate deployment — the AI is activated within existing OSS products.

---

## Confirmed results

| Operator | Vendor | Result | Methodology |
|----------|--------|--------|-------------|
| Elisa Finland | Nokia MIKA | ~15% capex efficiency | Compare AI-flagged sites vs. non-flagged; early intervention vs. reactive expansion |
| Tele2 Sweden | Nokia MIKA | ~12% capex efficiency | Similar methodology |

**Read these carefully:**
- "Capex efficiency" means fewer reactive upgrades — not a 15% budget reduction
- Both deployments are Nordic tier-1 operators with mature OSS data hygiene — favorable conditions
- Calibrated range for operators with clean PM data: **8–15% capex efficiency improvement**
- Operators with fragmented or multi-vendor PM data: expect lower gains until data normalization is in place

---

## What this use case is not

- Not real-time optimization — the control loop is days/weeks, not milliseconds
- Not closed-loop — a human approves every capacity change
- Not a substitute for site planning (it optimizes existing assets, doesn't decide new site locations)
- Not AI in RAN — the model never touches live RAN parameters directly

---

## Practical checklist before deploying

- [ ] Is PM data being collected consistently across all vendors in the network?
- [ ] Are counter definitions aligned across vendors? (Nokia and Ericsson define "PRB utilization" differently)
- [ ] Does the quarterly capex review process have a place for a data-driven input?
- [ ] Is the OSS vendor's AI module already licensed (often included, not activated)?

**The most common finding in RAN AI maturity assessments:** Capacity planning AI is already included in the operator's existing OSS contract but has never been activated. Check this before purchasing anything new.

---

## Failure modes specific to this use case

**Model blind spots:**
- Unplanned events (stadium opening, new residential development) — model extrapolates trend, misses step-change
- Multi-vendor PM counter inconsistency corrupts the training data (FM-1)
- After a major software upgrade, historical traffic patterns may no longer predict future patterns (FM-7)

**Organizational failure:**
- AI output is generated but not integrated into the capex review process — it becomes a dashboard no one checks
- Planning engineers distrust the model after one significant miss and revert to manual trend analysis (FM-5 dynamic in a planning context)
