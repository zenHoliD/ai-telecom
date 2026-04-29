# Sources

Quality tiers: **[OP]** operator case study · **[IND]** independent analyst · **[STD]** standards body · **[VND]** vendor

---

## Operator case studies

| # | Source | Key finding | Used in |
|---|--------|-------------|---------|
| 1 | Ericsson / Vodafone UK — 5G energy trial (2024) **[OP]** | 14% network-wide savings; up to 33% at select London sites | energy-optimization |
| 2 | Rakuten Mobile / Symphony — production O-RAN (2025) **[OP]** | Production xApps confirmed; domain isolation for conflict; 4–6 months per custom xApp | o-ran-xapps, failure-modes |
| 3 | DISH / EchoStar — US greenfield O-RAN (2022–2024) **[OP]** | FM-8 at scale; AI-native slippage; $15B+ cost overrun; 18–24 months integration delay | failure-modes |

---

## Independent analysts

| # | Source | Key finding | Used in |
|---|--------|-------------|---------|
| 4 | RCR Wireless — AI RAN energy (Oct 2024) **[IND]** | Industry range 7–30% energy savings; 4 operator case studies with methodology notes | energy-optimization |
| 5 | NVIDIA State of AI in Telecom 2026 **[IND]** | 1,000+ telco survey; predictive maintenance #1 use case; configuration drift top concern | predictive-fault, roi-framework |
| 6 | Analysys Mason — Open RAN deployments (Apr 2024) **[IND]** | Three-category deployment split (hardware / RIC trial / RIC primary); O-RAN TCO model | o-ran-xapps, roi-framework |
| 7 | McKinsey — AI value in telecommunications (2024) **[IND]** | Two-tier market; ROI leaders vs. laggards; three ways ROI models break | roi-framework, failure-modes |
| 8 | GSMA Intelligence — AI in mobile networks (2024) **[IND]** | Realized ROI is 50–70% of projected; operator segmentation; 74% capacity planning adoption | roi-framework, business-cases |
| 9 | Gartner — AI-augmented network operations (2023–2024) **[IND]** | Primary source for 40% MTTR claim; conditions required; 67% hit false positive crisis in 90 days | predictive-fault, roi-framework |
| 10 | Heavy Reading / 5G World Pro — O-RAN deployment reality (Mar 2026) **[IND]** | AT&T, DT, Vodafone, Airtel confirmed production; xApps in trial for most; ~13% global production O-RAN | o-ran-xapps, landscape |
| 11 | AI-RAN Reality Check 2026 **[IND]** | No production AI-RAN deployments; standards gap; workforce readiness primary bottleneck | failure-modes, landscape |
| 12 | IEEE ComSoc — AI in 3GPP RAN Release 18 **[IND]** | Beam management feasibility confirmed; positioning improvements; protocol changes needed for standardization | beam-management |
| 13 | IEEE / O-RAN WG3 — xApp conflict management (2024–2025) **[IND]** | Three conflict types; WG3 solution specified but not widely implemented; MARL in simulation only | o-ran-xapps, failure-modes |
| 14 | NOC AI — predictive fault detection synthesis **[IND]** | MTTR reduction mechanism; predictive vs. reactive detection patterns; operator adoption dynamics | predictive-fault |
| 15 | 5G Americas — AI and automation in 5G/6G RAN (Oct 2024) **[IND]** | 5-stage automation model; SON-to-AI history; two market gap observations | landscape, o-ran-xapps, business-cases |
| 16 | Nokia Bell Labs / Heavy Reading — capacity planning AI (2023–2024) **[VND+IND]** | 8–15% capex efficiency; highest adoption use case (74%); embedded in existing OSS | capacity-planning |

---

## Standards bodies

| # | Source | Key finding | Used in |
|---|--------|-------------|---------|
| 17 | 3GPP AI/ML Roadmap Rel-15→21 **[STD]** | Full standardization timeline; 3GPP vs. O-RAN scope boundary; spec-to-product pipeline | beam-management, o-ran-xapps, landscape |
| 18 | TM Forum Autonomous Networks L0–L5 (IG1230/IG1251) **[STD]** | Industry-standard vocabulary for autonomy levels; L2→L3 transition as primary barrier | roi-framework, o-ran-xapps |
| 19 | O-RAN Alliance architecture and interface specs (WG1/WG2/WG3) **[STD]** | Near-RT / Non-RT RIC split; xApp / rApp scope; E2 / A1 / O1 interface definitions | o-ran-xapps |

---

## Vendor

| # | Source | Key finding | Used in |
|---|--------|-------------|---------|
| 20 | Ericsson / Nokia / Huawei — beam management proprietary AI **[VND]** | What ships today vs. what 3GPP will standardize; proprietary vs. open-interface gap | beam-management |
| 21 | RAN AI failure patterns synthesis **[IND]** | FM-1 through FM-8 taxonomy; severity matrix; trial-to-production gap as leading failure | failure-modes |

---

## Notes on source quality

**What to trust for quantitative claims:**
- Energy savings: trust [OP] operator case studies with named operators and disclosed methodology. Discount vendor-only numbers by 30–50%.
- MTTR reduction: the 40% figure is Gartner median for human-in-loop AI (#9). Closed-loop automation achieves 71% — but that requires Stage 4+ autonomy.
- O-RAN adoption: the 13% figure is O-RAN Alliance self-report; Analysys Mason's three-category split (#6) is more useful than a single percentage.

**What to treat as directional only:**
- Market size projections (Grand View Research, Dell'Oro): useful for context, not for business cases
- GSMA Intelligence adoption percentages (#8): self-reported by operators; "some deployment" overstates production maturity

**Primary source gaps (as of April 2026):**
- 3GPP TR 38.843 full text — free at 3gpp.org, not yet synthesized in study plan
- O-RAN Alliance WG2/WG3 full specs — free with registration at o-ran.org
- Gartner full report — paywalled; #9 in this table is a synthesis of key findings
