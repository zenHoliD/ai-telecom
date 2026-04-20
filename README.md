# AI in RAN — Signal vs. Hype Guide

A personal reference for understanding where AI is actually working in RAN
today, built from operator evidence rather than vendor marketing.

**Author:** RAN engineer, inside the industry  
**Started:** April 2026  
**Status:** In progress

---

## What this is

Most AI-in-telecom content is vendor decks. This isn't.

This guide answers one question: **what's real, what's 18 months away, and
what's still theater?** It's built for someone who already knows RAN and
wants an honest map of the AI landscape — not a list of use cases, but a
framework for evaluating which ones are worth their time.

---

## How to use it

**Start here:** [`01-landscape.md`](01-landscape.md) — the maturity map.
Everything else branches from it.

If you're evaluating a specific use case, go to [`02-use-cases/`](02-use-cases/).
Each file covers one use case: what's deployed, what the numbers actually show,
and what the vendor claims leave out.

If you're building a business case, use [`03-roi-framework.md`](03-roi-framework.md)
to evaluate any use case on 5 axes before committing.

If a project is already failing or you want to avoid a common failure pattern,
go to [`04-failure-modes.md`](04-failure-modes.md).

Sources and quality ratings for all claims: [`sources.md`](sources.md).

---

## Contents

| File | What it covers | Status |
|------|---------------|--------|
| [`01-landscape.md`](01-landscape.md) | Maturity map — deployed, trial, hype | 🔲 To write |
| [`02-use-cases/energy-optimization.md`](02-use-cases/energy-optimization.md) | Sleep modes, carrier shutdown, real results | 🔲 To write |
| [`02-use-cases/beam-management.md`](02-use-cases/beam-management.md) | Intelligent cell shaping, beamforming AI | 🔲 To write |
| [`02-use-cases/predictive-fault.md`](02-use-cases/predictive-fault.md) | Fault prediction, NOC/SOC automation | 🔲 To write |
| [`02-use-cases/o-ran-xapps.md`](02-use-cases/o-ran-xapps.md) | O-RAN architecture, xApps, rApps, reality vs. roadmap | 🔲 To write |
| [`03-roi-framework.md`](03-roi-framework.md) | 5-axis framework for evaluating any use case | 🔲 To write |
| [`04-failure-modes.md`](04-failure-modes.md) | Why most RAN AI projects fail | 🔲 To write |
| [`05-business-cases.md`](05-business-cases.md) | Gaps and opportunities — fill in after 01-04 | 🔲 Placeholder |
| [`sources.md`](sources.md) | All references with quality ratings | 🔲 To write |
| [`design.md`](design.md) | Project framing and premises | ✅ Done |

---

## The core finding (April 2026)

Three tiers of maturity in RAN AI right now:

**Tier 1 — Deploy now, ROI is real:**
- Energy optimization (sleep modes, carrier shutdown) — 7-30% energy savings,
  weeks to deploy, multiple operator references

**Tier 2 — Real results, longer cycle:**
- Beam management / intelligent cell shaping — 5-30% throughput gains,
  months to deploy, vendor-specific
- Predictive fault detection — proven in NOC/SOC, broad adoption

**Tier 3 — Trial stage, 2027-2028 production horizon:**
- O-RAN + xApps in production (~13% operators today)
- AI-native RAN at edge (MWC 2026 demos, not at scale)
- Closed-loop autonomous network functions

**Not ready:**
- Fully autonomous RAN (no human oversight)
- GenAI for real-time network management
- 6G AI-native architecture (2029+ horizon)

---

## How this evolves

This is a living document. The landscape changes fast — anything written in
2025 is partially wrong today.

Update cadence:
- **After major industry events** (MWC, ONS, 3GPP plenary): update the
  maturity map in `01-landscape.md`
- **After a new operator case study drops**: add to the relevant use-case
  file and `sources.md`
- **After a project at work**: add a failure mode or business case entry

When at least one colleague finds this useful without being prompted to read
it, it's ready to share more broadly.
