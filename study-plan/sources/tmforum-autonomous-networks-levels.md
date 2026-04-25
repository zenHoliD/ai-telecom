# TM Forum — Autonomous Networks Framework (L0–L5)

**Source:** TM Forum IG1230 "Autonomous Networks: Empowering Digital Transformation" + supporting white papers
**URL:** https://www.tmforum.org/resources/ig1230/ (registration required)
**Supporting:** IG1231, IG1232 (implementation guidance); IG1251, IG1252 (intent-driven operations)
**Quality tier:** [STD] Standards body — industry consensus framework, widely cited by operators
**Relevant to:** `03-roi-framework.md`, `02-use-cases/o-ran-xapps.md`

---

## Why this matters for the guide

Every ROI conversation about RAN AI eventually maps to autonomy levels. Operators use L0–L5 to communicate their automation roadmap to investors, set internal project targets, and evaluate vendor claims. The NVIDIA survey found 88% of operators at L1–L3 — this framework is what that statistic means.

---

## The L0–L5 autonomy levels

| Level | Name | Human role | Control loop | Network coverage |
|-------|------|-----------|--------------|-----------------|
| L0 | Manual | Executes all tasks | None | N/A |
| L1 | Assisted | Decides all actions; system executes routine tasks | Open-loop, recommendations only | Per-element |
| L2 | Partial | Reviews high-risk decisions; approves automated actions | Closed-loop within domain, human gate on exceptions | Single domain |
| L3 | Conditional | Monitors flagged exceptions; system handles normal scenarios autonomously | Closed-loop, cross-domain with human oversight | Cross-domain |
| L4 | High | Handles only unanticipated edge cases | Closed-loop, self-healing, exception-based human input | Network-wide |
| L5 | Full | No intervention required | Fully autonomous, self-optimizing, self-learning | Network-wide |

**Current industry position (April 2026):** Most operators are at L2–L3. L4 is the stated 5-year target for leading operators (Deutsche Telekom, SK Telecom, China Mobile). L5 is aspirational — no timeline.

---

## Key concepts

### Intent-driven operations
Above L3, the paradigm shifts from task-driven ("turn on sleep mode for sector X") to intent-driven ("maintain 99.95% availability while minimizing energy"). The network autonomously selects and executes actions to fulfill the intent. TM Forum's IG1251/1252 documents model how operators express intent and how networks interpret it.

### Closed-loop control as the L2→L3 transition
The critical jump is from open-loop (AI recommends, human approves) to closed-loop (AI decides and acts). This is exactly the distinction explored in the energy optimization and beam management sources:
- Energy optimization deployments often start at L2 (open-loop), move to L3 (closed-loop overnight, open-loop peak hours)
- O-RAN xApps enable L2–L3 depending on the xApp design

### "Zero-X" target
TM Forum's stated ambition: "zero-touch, zero-wait, zero-trouble" networks by 2025. That target slipped — the workforce readiness and vendor interoperability problems documented in the O-RAN deployment reality source are a direct contributor.

---

## L0–L5 mapped to the use cases in this guide

| Use case | Current level | Ceiling today | What's blocking L4 |
|----------|--------------|---------------|---------------------|
| Energy optimization | L2–L3 | L3 (closed-loop, scoped hours) | QoS regression risk triggers operator caution |
| Beam management | L1–L2 | L2 (Rel-18 study, no production L3) | 3GPP standardization not complete until Rel-20 |
| Predictive fault detection | L1–L2 | L2 (alert generation, human response) | NOC integration, alert fatigue, liability questions |
| O-RAN xApps | L2–L3 | L3 (single-domain, scoped) | Multi-vendor coordination, xApp conflict management |

---

## What to extract for roi-framework.md

- The L0–L5 table is a ready-made dimension for the deployment speed / automation level axes
- "Intent-driven" framing is useful for distinguishing RAN AI that delivers durable value vs. one-off trials
- The gap between L3 and L4 is where most operator business cases live — this is the ROI sweet spot to analyze
- Use L-level transitions as a proxy for deployment cycle: L1→L2 = weeks, L2→L3 = months, L3→L4 = multi-year program

---

## Skepticism note

TM Forum is a trade association funded by telcos and vendors. The L0–L5 framework is useful structurally but the "path to L5" vision serves member vendors as much as operators. The 2025 "zero-X" target clearly slipped — treat TM Forum timeline projections with skepticism. The framework is more useful as a shared vocabulary than as a deployment roadmap.
