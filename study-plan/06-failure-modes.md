# Reading: Failure Modes

*Prepares: `04-failure-modes.md`*

## Note on sourcing

This section is where your insider operator experience is the highest-quality
source. Published failure postmortems are rare in telecom — operators don't
advertise what went wrong. Treat external sources as a checklist to validate
and extend your own observations, not as the primary input.

## External sources

- **[IND]** TM Forum: "Autonomous Networks: lessons from early deployments"
  Search: "TM Forum autonomous networks lessons learned operators"
  Use for: documented patterns from operators who've tried and stalled.

- **[IND]** Heavy Reading / Analysys Mason: O-RAN deployment challenge reports
  These regularly cover why O-RAN pilots don't reach production.

- **[IND]** IEEE: Academic papers on ML deployment failures in network management
  Search: "machine learning deployment failures network management IEEE"
  More honest about failure modes than vendor materials.

- **[IND]** McKinsey: "Why AI transformations fail" (general, not telecom-specific)
  Search: "McKinsey why AI projects fail"
  Use for: general patterns that apply to RAN AI (data quality, org change, etc.)

## RAN-specific failure categories to cover

These are hypotheses — confirm or refute from your own experience:

1. **Data quality** — KPI counters aren't designed for ML training; missing data, vendor-specific counter definitions, clock skew
2. **Trial-to-production gap** — pilots succeed in controlled conditions; production breaks on edge cases
3. **Vendor lock-in** — AI feature requires proprietary data stream unavailable to third parties
4. **xApp conflict** — two optimization loops fight each other (energy vs. throughput)
5. **Org readiness** — NOC teams don't trust closed-loop; revert to manual after first incident
6. **Baseline problem** — can't prove the AI helped because there's no clean A/B test in a live network
7. **Model drift** — traffic patterns change seasonally; model trained on last year's data degrades

## What to extract

- [ ] Which failure modes are most common in your org's experience?
- [ ] Which are solvable with better process vs. fundamental technical constraints?
- [ ] For each mode: what's the early warning sign that a project is heading there?
