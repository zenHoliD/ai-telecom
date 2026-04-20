# Reading: Predictive Fault Detection

*Prepares: `02-use-cases/predictive-fault.md`*

## Primary sources

- **[IND]** NVIDIA: "State of AI in Telco" survey 2026
  Search: "NVIDIA state of AI in telecom survey 2026"
  Key stat: broad operator adoption cited. Look for: % operators deployed, use case breakdown.

- **[STD]** TM Forum: IG1230 — Autonomous Networks technical report
  Search: "TM Forum IG1230 autonomous networks"
  Use for: autonomy level definitions (L0–L5), where fault detection sits today.

## Secondary sources

- **[IND]** Analysys Mason: AI in telecom operations reports
  Search: "Analysys Mason AI telecom operations 2024 2025"
  Use for: independent market sizing and operator adoption rates.

- **[VND]** Ericsson / Nokia / Huawei NOC automation case studies
  Search: "[vendor] predictive maintenance RAN NOC automation case study"
  Use for: MTTR reduction claims, truck roll avoidance numbers.

- **[IND]** Heavy Reading: "AI-driven network operations" tracker
  Use for: vendor landscape and operator deployment status.

## What to extract

- [ ] Which failure types are being predicted well (hardware vs. software vs. interference)?
- [ ] False positive rates — operators care about NOC alert fatigue
- [ ] Lead time: how far in advance does the model flag issues?
- [ ] Integration point: does it plug into existing OSS/ticketing systems?
- [ ] Closed-loop or open-loop at production operators?
- [ ] Any comparison to threshold-based SON alarms (the baseline it replaces)
