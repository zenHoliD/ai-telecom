# McKinsey — AI Value and ROI in Telecommunications

**Source:** McKinsey Global Institute — "The State of AI" (annual series) + telecom-specific research
**Primary URL:** https://www.mckinsey.com/industries/technology-media-and-telecommunications/our-insights/the-state-of-ai-in-telecommunications
**Supporting:** McKinsey "Rewired: The McKinsey Guide to Outcompeting in the Age of Digital and AI" (2023)
**Quality tier:** [IND] Independent analyst — non-vendor, but consulting firm with commercial interests in AI transformation engagements
**Relevant to:** `03-roi-framework.md`, `04-failure-modes.md`

---

## Core framing

McKinsey's telecom AI research consistently identifies a two-tier industry:

> "A small group of AI leaders captures the majority of realized value. The rest are experimenting at scale without yet generating enterprise-level returns."

This framing is directly useful for the ROI framework — the question isn't "does RAN AI work" but "which operators are capturing value and why."

---

## Where the value actually is (by use case priority)

McKinsey ranks telco AI use cases by value-capture potential:

1. **Network operations automation** — highest OpEx reduction potential
   - Fault prediction + resolution: 10–25% MTTR reduction at operators with mature data platforms
   - Capacity optimization: 5–15% capex avoidance through more accurate demand prediction
   - Energy optimization: directly quantifiable, best short-term ROI

2. **Customer operations** — high volume, measurable, but not the guide's focus
   - Churn prediction, next-best-offer, automated care

3. **Revenue assurance / fraud** — fast payback, typically 6–18 month cycles

**Key insight for the guide:** Network operations AI has the clearest quantifiable ROI but requires the most mature data infrastructure. Customer AI is easier to deploy but lower relevance to RAN.

---

## What separates AI leaders from the rest

McKinsey identifies three structural differences at operators that have scaled AI:

### 1. Unified data platform
AI leaders built a centralized, governed data lake before deploying models. Laggards try to train models on fragmented OSS/BSS data and encounter the problems documented in the NOC AI and O-RAN deployment sources (vendor-specific counter definitions, clock skew, missing KPIs).

### 2. Executive sponsorship + dedicated AI team
Not "a data science team in IT" but a cross-functional unit with direct access to network engineering, product, and finance. McKinsey calls this an "AI factory" — a small, empowered group that moves faster than traditional IT project cycles.

### 3. Fast-fail portfolio approach
Leaders run many small AI experiments with short-cycle evaluation (8–12 weeks) rather than one large transformation program. This is the opposite of how most network modernization programs are structured — and explains why traditional telco project management cultures struggle.

---

## ROI benchmarks (ranges across operator studies)

| Use case | McKinsey value estimate | Payback period | Evidence quality |
|----------|------------------------|----------------|-----------------|
| Energy optimization | 10–30% energy cost reduction | 6–18 months | High — multiple operator case studies |
| Predictive fault detection | 10–25% MTTR reduction, 5–15% truck roll avoidance | 12–24 months | Medium — depends on data maturity |
| Capacity optimization | 5–15% capex avoidance | 18–36 months | Medium — hard to isolate AI contribution |
| Autonomous configuration | Difficult to quantify directly | Multi-year | Low — few operators at L3+ |

**Note:** McKinsey's numbers are typically ranges derived from case studies with selection bias — these are outcomes at operators who succeeded, not averages across all deployments.

---

## Where ROI models break down

McKinsey's research on failed AI programs (not telecom-specific but applicable) identifies three recurring failure patterns:

### 1. The baseline problem
> "If you can't measure the counterfactual, you can't prove the AI helped."

Live networks never have a clean control group. Operators often attribute improvements to AI that are actually caused by simultaneous network upgrades, seasonal traffic changes, or maintenance work. This inflates early ROI claims and creates credibility problems when auditors look closer.

### 2. Data readiness underestimated by 3–5x
Organizations consistently underestimate how long it takes to clean, label, and govern data for model training. McKinsey's research suggests data preparation consumes 60–80% of total AI project effort — the model itself is the easy part.

### 3. The adoption gap
Technical deployment ≠ value capture. McKinsey consistently finds that the largest gap between AI project investment and ROI realization is organizational: NOC teams don't use the alerts, engineers revert to manual workflows, and AI recommendations are ignored during high-stakes events (exactly the trust problem documented in the failure modes reading guide).

---

## Framework axes from McKinsey

These map directly to the 5-axis ROI framework in `03-roi-framework.md`:

| McKinsey dimension | Corresponding ROI axis |
|--------------------|----------------------|
| Evidence quality (case study vs. trial) | Evidence quality axis |
| Time-to-value (weeks vs. years) | Deployment speed axis |
| Automation level (L1 vs. L3) | Automation level axis |
| Data readiness requirements | Data/integration cost axis |
| Scaling potential (site vs. network-wide) | Scalability axis |

The five axes in the design.md framework map almost exactly to McKinsey's dimensions — this validates the framework structure.

---

## Skepticism note

McKinsey is in the business of selling AI transformation engagements to telcos. Their research publications serve as marketing for those engagements. ROI estimates are drawn from their client base (i.e., operators who hired McKinsey and therefore skew toward larger, more sophisticated operators). Numbers like "10–25% MTTR reduction" should be treated as upper-bound outcomes at mature operators, not realistic expectations for a first deployment.
