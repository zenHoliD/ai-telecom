# Reading: Beam Management

*Prepares: `02-use-cases/beam-management.md`*

## Primary sources

- **[STD]** 3GPP TR 38.843 — AI/ML for NR air interface (Rel-18 study item)
  Available free at: 3gpp.org, search "TR 38.843"
  Focus on: §6.2 beam management use case, evaluation results, identified gaps.
  Key finding: feasibility confirmed, protocol changes needed for standardization.

- **[VND]** Ericsson blog: "AI-powered RAN" (Jan 2023)
  Search: "Ericsson AI powered RAN blog January 2023"
  Key stats: 5% DL throughput ↑, 30% UL throughput ↑. Note: which operator, which bands?

## Secondary sources

- **[STD]** 3GPP TR 38.859 — Study on enhancements for AI/ML-based UE (Rel-19 input)
  Context for where beam management standardization is heading.

- **[VND]** Nokia: "AI/ML for beam management" technical papers
  Search: "Nokia AI beam management 5G massive MIMO"
  Use for: comparing implementation approach to Ericsson.

- **[IND]** IEEE ComSoc: "Overview of AI in 3GPP RAN Release 18"
  Search: "IEEE ComSoc AI 3GPP RAN Release 18 overview"
  Good for: independent read on what Rel-18 actually proved vs. what vendors claim.

## What to extract

- [ ] Confirmed operator deployments (not just lab results)
- [ ] Which deployment model: AI on RAN, AI+RAN, or AI in RAN?
- [ ] Hardware dependencies (massive MIMO only? specific bands?)
- [ ] Proprietary vs. standards-aligned implementation
- [ ] Gap between Rel-18 study item and what's in vendor products today
- [ ] What "30% UL improvement" baseline was — dense urban? Indoor? What traffic load?
