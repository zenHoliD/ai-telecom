# Business Cases — Opportunity Gaps in RAN AI

*Last updated: April 2026*
*Status: Scaffold — fill in as evidence accumulates*

---

## How to use this file

This is not a use-case catalog. Vendors produce those for free. This file tracks specific gaps where:
1. There is clear operator demand
2. The current supply (vendor products, internal capabilities) is insufficient or immature
3. There is a plausible path to a product or service that captures the value

Add an entry only when you have evidence of the gap — not when you have a hypothesis.

---

## Gap 1: Neutral xApp certification and testing

**The problem:**
No vendor-neutral lab exists for testing xApps across multiple RIC vendors and multiple RAN vendor stacks. An operator deploying multi-vendor O-RAN must do this testing themselves, which requires:
- A full O-RAN stack (RU + DU + CU + Near-RT RIC + Non-RT RIC) in a test environment
- Expertise across all vendor stacks being tested
- A repeatable test suite for each E2 service model

This is expensive to build and maintain. Most operators don't have it.

**Evidence of demand:**
5G Americas (Oct 2024) names this explicitly as an unmet industry need. O-RAN Alliance's TIFG (Testing and Integration Focus Group) is working on specifications, but no commercial neutral lab is established at scale.

**Who currently plays here:**
- Keysight Technologies and Spirent offer O-RAN test equipment
- VIAVI Solutions has O-RAN test tools
- No one offers an end-to-end xApp certification service at neutral scale

**Possible business case:**
A vendor-neutral O-RAN test and certification lab, either as a standalone service or embedded in an existing neutral host / managed service provider. Could be structured as a per-xApp certification fee (like Wi-Fi Alliance certification) or as a subscription lab access model.

**What would need to be true:**
- O-RAN Alliance standardizes the certification criteria (in progress)
- Enough xApp vendors to justify the setup cost
- Operators trust the certification (requires independence from RAN vendors)

**Evidence to gather:**
- [ ] How many operators are actively blocking xApp deployments due to lack of test infrastructure?
- [ ] What is the TIFG timeline for certification specifications?
- [ ] Is any GSMA or O-RAN Alliance member building this internally?

---

## Gap 2: RAN-specific MLOps platform

**The problem:**
Standard MLOps tooling (MLflow, Kubeflow, AWS SageMaker) was built for enterprise AI, not telecom network AI. RAN-specific requirements not met by generic platforms:
- A/B testing at cell level (requires traffic steering to randomize treatment/control)
- Model drift detection tied to RAN KPI semantics (PRB utilization, RSRP, SINR)
- Rollback procedures that interact with RAN configuration management (not just model rollback)
- Retraining triggered by network events (topology changes, software upgrades)
- Multi-vendor data normalization as a first-class pipeline step

Operators building xApps have to build this MLOps layer themselves — every operator is solving the same problem independently.

**Evidence of demand:**
5G Americas (Oct 2024) names this as an unmet need. FM-7 (model drift) in the failure modes taxonomy is partially caused by the absence of production-grade MLOps for RAN — operators don't know when their models degrade.

**Who currently plays here:**
- Ericsson and Nokia embed some MLOps capability in their AI products (proprietary, locked to their stack)
- NVIDIA has telecom-specific AI tools (not RAN MLOps specifically)
- No independent, multi-vendor RAN MLOps platform exists

**Possible business case:**
An MLOps platform built for multi-vendor RAN: data normalization layer, cell-level A/B test infrastructure, RAN-aware drift detection, and rollback procedures integrated with standard OSS configuration management systems.

**What would need to be true:**
- Enough operators building custom xApps to need this (currently small — mostly tier-1)
- Standard data models (O-RAN O1 counters) mature enough to abstract vendor differences
- Operators willing to pay for a platform vs. building their own

**Evidence to gather:**
- [ ] How many tier-1 operators currently have in-house xApp development programs?
- [ ] What MLOps tooling are Rakuten and T-Mobile US using for xApp management?
- [ ] Is O-RAN Alliance's AI/ML workflow group (WG2) standardizing any of this?

---

## Gap 3: Brownfield O-RAN migration consulting / tooling

**The problem:**
DISH proves greenfield O-RAN is hard. Brownfield (existing network, migrate to O-RAN) is harder. No operator has successfully migrated a large existing network from traditional RAN management to an O-RAN RIC-primary architecture at scale. The migration path involves:
- Running O-RAN management alongside traditional OSS/BSS for a transition period
- Migrating cell-by-cell or cluster-by-cluster without degrading live network
- Retiring proprietary management systems that operators have built workflows around for 10+ years

**Evidence of demand:**
Every tier-1 operator with O-RAN ambitions has this problem. AT&T, DT, and Vodafone are in Category A (open hardware) and need a path to Category C (RIC primary). No established migration playbook exists.

**Possible business case:**
Migration tooling or consulting practice specialized in brownfield O-RAN transition — managing the dual-mode operation period, traffic cutover procedures, and OSS/BSS workflow migration. This is the integration engineering gap that FM-8 points to.

**Evidence to gather:**
- [ ] What is Vodafone's published plan for moving from their current O-RAN hardware deployment to active RIC?
- [ ] Is any systems integrator (Accenture, IBM, Infosys) building a specialized O-RAN migration practice?
- [ ] What is the integration engineering cost as a % of total O-RAN program cost?

---

## Entries to add over time

- AI-based spectrum management / dynamic spectrum sharing (DSS) — when you have evidence of a specific gap
- AI for fronthaul / midhaul optimization — relevant when split architecture deployments grow
- Private network / enterprise RAN AI — smaller scale, faster feedback loop, different buyer
- Any gap you identify from direct operator experience (use `operator-experience.md` as the feed)
