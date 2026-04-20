# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a research documentation project — a structured, evidence-based guide to AI/ML maturity in Radio Access Networks (RAN). The goal is to separate what's actually deployed in production from what's still in trial or hype stages. All content is in Markdown.

## No Build System

There are no build, test, or lint commands. This is a pure documentation project.

## Document Structure

```
README.md              # Navigation index and core findings summary
design.md              # Project framing, premises, success criteria, roadmap
01-landscape.md        # Maturity map — vocabulary, compute models, deployment tiers (COMPLETE)
02-use-cases/          # Deep dives per use case (all stubs, to be written)
03-roi-framework.md    # 5-axis ROI evaluation framework (to write)
04-failure-modes.md    # Why RAN AI projects fail (to write)
05-business-cases.md   # Opportunity gaps (placeholder)
sources.md             # Quality-rated references (to write)
```

## Content Architecture

### Maturity Tiers (April 2026 snapshot)

- **Tier 1 (deploy now):** Energy optimization — 7–30% savings, weeks to deploy, multiple operator references
- **Tier 2 (real results, longer cycle):** Beam management (5–30% throughput), predictive fault detection
- **Tier 3 (trial stage, 2027–2028 horizon):** O-RAN xApps in production (~13% operators), AI-native RAN at edge
- **Not ready:** Fully autonomous RAN, GenAI for real-time network management

### Three Compute Models (vocabulary defined in 01-landscape.md)
- **AI on RAN** — model runs on RAN hardware itself
- **AI+RAN** — model runs externally, controls RAN via closed-loop
- **AI in RAN** — model embedded in RAN function (e.g., AI-native scheduler)

### Source Quality Hierarchy
Operator case study > Independent analyst > Vendor blog

### Vendor Claim Evaluation (5 questions from 01-landscape.md)
1. Which operator deployed this in production?
2. What was the baseline?
3. Was it closed-loop or open-loop?
4. What data did it require?
5. What happened after the trial?

## Writing Standards

- Evidence-first: every maturity claim needs an operator or independent analyst reference
- Distinguish production deployment from trial/pilot
- Flag vendor-specific results that may not generalize
- Update `sources.md` whenever a new reference is cited anywhere

## Update Cadence

- After major industry events (MWC, ONS, 3GPP plenary) → update `01-landscape.md`
- When new operator case studies are published → update relevant use-case file + `sources.md`
- After internal project experience → add entry to `04-failure-modes.md` or `05-business-cases.md`

## Key Domain Terms

Defined fully in `01-landscape.md`: SON, RIC, xApp, rApp, O-RAN, CU/DU split, near-RT RIC, non-RT RIC, SMO, A1/E2/O1 interfaces, closed-loop vs. open-loop control.
