# Analysys Mason — Open RAN Operator Deployments and Economics

**Source:** Analysys Mason, "Open RAN: operator deployments, business cases, and vendor ecosystem" (April 2024)
**Supporting:** Analysys Mason, "The business case for Open RAN" (2023), operator interviews
**Quality tier:** [IND] Independent analyst — Analysys Mason is among the most rigorous telecom analysts; their methodology includes direct operator interviews
**Relevant to:** `02-use-cases/o-ran-xapps.md`, `03-roi-framework.md`

---

## Analyst credibility note

Analysys Mason occupies a different quality tier from GSMA Intelligence for this topic. They conduct primary operator interviews (off-record) and validate quantitative claims against disclosed financial data. Their Open RAN research is frequently cited in operator investment cases and by regulators (Ofcom, BNetzA have both cited AM Open RAN reports).

---

## Deployment status (as of April 2024)

### The three-category split

Analysys Mason's core contribution is disaggregating "O-RAN deployment" into three distinct categories that the industry conflates:

**Category A: Open-hardware deployment**
- Disaggregated RU/DU/CU from same or different vendors
- Open Fronthaul interface (O-RAN Alliance spec)
- No RIC, no xApps, no real-time AI optimization
- *This is what AT&T, DT, and Vodafone have at scale*

**Category B: RIC-enabled deployment (trial/early production)**
- Near-RT RIC deployed alongside traditional RAN management
- 1–2 xApps running in non-critical domains (energy, coverage)
- RIC and RAN management coexist; RIC does not have primary control
- *This is where most advanced tier-1 operators are in 2024*

**Category C: Full RIC-primary architecture (production)**
- Near-RT RIC is the primary RAN optimization layer
- Multiple xApps running in production with active closed-loop control
- RAN management system handles only configuration and fault management
- *Only Rakuten Mobile and a small number of greenfield operators are here*

**Why this matters:** When operators say they "have O-RAN," they mean Category A. When the O-RAN Alliance shows xApp deployment statistics, they include Category B trials. Category C is what the AI xApp value proposition requires.

---

## Business case analysis

### Who is building a positive O-RAN ROI case?

Analysys Mason identifies three operator archetypes where O-RAN economics work:

**1. Greenfield/challengers in competitive markets**
- Lower cost per site vs. traditional vendor: 10–20% CAPEX savings when all-in-one traditional vendor is compared to disaggregated stack
- Faster deployment: Software-defined RAN enables faster parameter changes
- No legacy modernization cost: Starting fresh avoids the retrofit problem
- *Examples: Rakuten, DISH, 1&1 (Germany)*

**2. Rural coverage expansions in price-sensitive markets**
- Open RAN hardware from Tier-2 vendors is 15–30% cheaper than major vendor equivalent for rural macros
- Rural sites have simpler performance requirements — interoperability issues matter less
- *Examples: Airtel India rural, MTN Africa rural expansion*

**3. Operators with specific vendor-diversity mandates**
- National security concerns (some European operators)
- Regulatory diversity requirements
- Strategic balance of power vs. single-vendor dependence
- *Examples: Vodafone UK (political context post-Huawei exclusion)*

**Operators where O-RAN economics don't currently work:**
- Dense urban with existing investment in traditional RAN infrastructure
- Mid-band 5G dense deployment (most mature vendor stacks outperform current O-RAN implementations on capacity per watt)
- Operators that have just completed a major RAN modernization (sunken-cost pressure)

---

## Total cost of ownership: O-RAN vs. traditional RAN

Analysys Mason's TCO comparison (modeled for a 10,000-site European operator, 10-year horizon):

| Cost category | Traditional RAN | O-RAN |
|---------------|----------------|-------|
| CAPEX (hardware + software) | 100 (index) | 90–95 |
| Integration and deployment | 100 | 115–135 |
| OPEX (management, maintenance) | 100 | 95–105 (early) → 80–85 (steady state with automation) |
| Risk/failure cost | 100 | 120–140 (multi-vendor complexity) |

**Summary:** O-RAN has a CAPEX advantage (5–10%) and a potential long-run OPEX advantage (15–20% at steady state with AI automation). It carries higher integration cost and risk in the deployment phase.

**The steady-state OPEX advantage requires the AI/automation layer to work.** Without xApps delivering automation at scale, O-RAN's OPEX lands at parity with traditional RAN — the CAPEX discount is insufficient to justify the integration complexity alone.

---

## Most useful for roi-framework.md

The 5-axis ROI framework should include an "automation dependency" factor: O-RAN deployments that cannot realize the automation layer (Category B stuck at 1–2 xApps) will not achieve positive TCO vs. traditional RAN. The business case assumes Category C.

This is why FM-4 (xApp conflict) and FM-8 (integration complexity) are not just operational risks — they're business case risks. If you can't get to 5+ production xApps, the O-RAN ROI model doesn't close.
