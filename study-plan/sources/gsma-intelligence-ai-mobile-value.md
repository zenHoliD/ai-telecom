# GSMA Intelligence — The Value of AI in Mobile Networks

**Source:** GSMA Intelligence, "Artificial Intelligence in Mobile Networks: Quantifying the Opportunity" (2024)
**Supporting:** GSMA "State of Mobile Internet Connectivity 2024," operator interviews cited in the report
**Quality tier:** [IND] Independent analyst — GSMA Intelligence uses operator survey data, making this one tier below direct operator case study
**Relevant to:** `03-roi-framework.md`, `05-business-cases.md`

---

## What GSMA Intelligence is

GSMA Intelligence is the research arm of the GSMA (the industry body representing 750+ mobile operators). Their reports combine:
- Operator survey data (self-reported)
- Publicly disclosed financial data
- Proprietary forecasting models

The quality caveat: self-reported operator data may reflect aspirational rather than realized results. Treat with [IND] rigor — useful for sizing and direction, not for precise ROI claims.

---

## Top-line findings

### AI adoption is concentrated in network operations

When operators report AI deployment, the dominant use cases (by revenue-weighted adoption) are:

| Use case | % of operators with some deployment | Primary value driver |
|----------|-------------------------------------|---------------------|
| Predictive network maintenance | 68% | OPEX — truck roll reduction |
| Energy optimization | 61% | OPEX — energy costs |
| Traffic forecasting / capacity planning | 74% | CAPEX — RAN investment efficiency |
| Customer experience (churn prediction) | 53% | Revenue — retention |
| RAN parameter optimization | 34% | KPI — throughput, drop rate |

**Note:** "Some deployment" ranges from a pilot at 10 sites to a full network rollout. These numbers significantly overstate production maturity.

### Expected vs. realized value

This is the most actionable finding in the report:

| Use case | Expected ROI (at project start) | Realized ROI (post-deployment) | Gap |
|----------|--------------------------------|-------------------------------|-----|
| Energy optimization | 20–35% reduction | 10–25% reduction | ~10pp |
| Predictive maintenance | 40% MTTR reduction | 20–30% MTTR reduction | ~15pp |
| Traffic forecasting | 15% capex efficiency | 8–12% capex efficiency | ~5pp |

**The ROI gap is consistent and predictable:** realized value is typically 50–70% of the projected value. Operators who account for this in their business cases have a more accurate model.

### Why the gap exists (GSMA's analysis)

GSMA identifies three structural causes:
1. **Baseline problem** — operators measure improvement against their "current" state, not a rigorously controlled baseline. Confounding variables (network upgrades, traffic growth) inflate perceived AI impact.
2. **Scope creep in the denominator** — integration costs and data preparation costs are often excluded from ROI calculations at project start, then absorbed by IT budgets.
3. **Adoption rate shortfall** — AI is deployed but underused. NOC teams revert to manual processes after any significant incident.

---

## Operator segmentation: AI leaders vs. the rest

GSMA segments operators into three tiers based on AI maturity:

**AI Leaders (~15% of operators by revenue):**
- Unified data platform across network domains
- AI-native operations (L3+ autonomy for defined use cases)
- In-house AI team embedded in network engineering
- Examples: Rakuten, Reliance Jio, T-Mobile US, SK Telecom

**AI Followers (~45% of operators):**
- AI deployed in 2–3 use cases with vendor solutions
- Human-in-the-loop for all consequential decisions
- AI viewed as a product feature, not a platform capability
- Examples: Most tier-1 European operators (Vodafone, DT, Orange in current state)

**AI Laggards (~40% of operators):**
- AI limited to analytics and dashboards
- No closed-loop automation
- Typically tier-2/3 operators by revenue, or markets with limited data infrastructure

**ROI implication:** The 7–30% energy savings range in the market is real — but the 30% end is achieved by AI Leaders with mature data platforms. An AI Laggard implementing the same vendor product will see results closer to 7–12%.

---

## GSMA's 5-year forecast for network AI value

- $160B in cumulative value creation from network AI, 2024–2029 (GSMA estimate)
- 60% of this value comes from OPEX reduction (energy, maintenance, workforce)
- 40% from CAPEX efficiency (better capacity planning, fewer speculative upgrades)

*Use this for context in business cases, not as a hard number. GSMA forecasts consistently overestimate near-term AI value in network domains.*

---

## Most useful for roi-framework.md

The GSMA finding that realized ROI is 50–70% of projected ROI is the most important calibration input for the 5-axis framework. Build this discount in explicitly:

> When an operator presents a business case for RAN AI, apply a 50–70% realization discount to expected savings. Require a clear baseline methodology before accepting any KPI improvement claim.
