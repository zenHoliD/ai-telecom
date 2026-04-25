# 3GPP AI/ML Standardization Roadmap — Rel-15 Through Rel-21

**Source:** 3GPP Release descriptions (3gpp.org), IEEE ComSoc "AI in 3GPP" survey, public 3GPP status pages
**Direct URL:** https://www.3gpp.org/specifications-technologies/releases
**Quality tier:** [STD] Standards body — factual release timeline, no vendor interest
**Relevant to:** `02-use-cases/beam-management.md`, `02-use-cases/o-ran-xapps.md`, `01-landscape.md`

---

## Why a single timeline source matters

AI/ML work in 3GPP is spread across multiple releases, multiple working groups (RAN, SA, CT), and multiple TR/TS documents. The IEEE ComSoc Rel-18 source covers the air interface in depth. This source zooms out: **what got standardized when, what's coming, and what's the gap between a 3GPP spec and a commercial product.**

---

## 3GPP release cadence

3GPP releases approximately every 12–18 months. The naming follows a freeze schedule:

- **Stage 1 freeze** (feature requirements) → **Stage 2 freeze** (architecture) → **Stage 3 freeze** (protocol specs) → **ASN.1 freeze** (encoding) = "Release complete"
- Commercial products typically appear **18–24 months after Stage 3 freeze**
- Interoperability testing adds 6–12 months beyond that

This means: a feature standardized in Rel-19 (Stage 3 freeze ~2026) reaches multi-vendor production at scale in **2027–2028**.

---

## AI/ML by release

### Rel-15 (2018) — NR foundation, AI analytics enters core
- 5G NR defined; no AI in RAN air interface
- **NWDAF** (Network Data Analytics Function) introduced — AI analytics for core network only (subscriber behavior, QoS prediction, slice usage)
- Baseline: all RAN AI in Rel-15 era is proprietary vendor SON, not standardized

### Rel-16 (2020) — SON enhancements, NWDAF expanded
- Handover optimization improvements (SON)
- NWDAF service exposure to third-party applications
- **No AI in the NR air interface** — still purely core-side

### Rel-17 (2022) — NWDAF at scale, first RAN AI study items
- NWDAF model training / inference separation
- **TR 37.817**: First study on AI/ML for NG-RAN (network side analytics) — study item only, no normative specs
- O-RAN Alliance AI work runs in parallel but is not 3GPP — this is a common source of confusion

### Rel-18 (2024, "5G Advanced") — First AI in NR air interface
This is the pivotal release. See `ieee-comsoc-ai-3gpp-rel18.md` for full detail.

**TR 38.843** (study item, now informing Rel-19 work items):
- Beam management via AI/ML: feasibility confirmed, protocol gaps identified
- CSI feedback compression/prediction: two-sided UE+gNB model studied
- Positioning accuracy enhancement: NLOS detection and correction

**Framework established:**
- 5 logical AI functions (data collection, training, management, inference, storage)
- Model lifecycle management (activation, switching, fallback)
- Key requirement: AI must not degrade below legacy baseline → mandatory fallback

**What Rel-18 did NOT do:** Did not produce normative specs for the air interface AI use cases. They remain study items that feed Rel-19.

### Rel-19 (estimated Stage 3 freeze: 2026) — Normative AI air interface
- **CSI prediction** promoted to Work Item (normative): single-sided model (4ms/8ms horizon) and two-sided model
- **Beam management** AI: normative work item, target for multi-vendor interoperability specifications
- **Positioning accuracy**: Work Item for NLOS scenarios
- AI model transfer between UE and network
- **AI/ML for scheduling and link adaptation**: study items beginning

**Commercial reality:** Rel-19 specs finalizing in 2026 → vendor implementation 2026–2027 → interoperability testing → multi-vendor commercial availability **2027–2028**

### Rel-20 (estimated 2027–2028) — 6G foundation + AI-native RAN
- 6G (IMT-2030) preparatory work begins
- AI-native RAN architecture study items: AI embedded at the PHY layer, AI-based scheduler design
- **AI-RAN** as a concept appears in 3GPP for the first time as a study item (not yet specifiable)
- Massive MIMO evolution with AI-assisted beamforming as a first-class feature

### Rel-21 (estimated 2029+) — 6G normative
- First 6G normative specifications
- AI-native interfaces and protocols if feasibility proven in Rel-20 study phase
- This is where "AI in RAN" (model embedded in RAN function, not external) becomes standardizable

---

## The spec-to-product pipeline (illustrated)

```
Rel-18 study items (2024)
    ↓ 18 months
Rel-19 normative specs (2025–2026)
    ↓ 18–24 months vendor implementation
Commercial single-vendor products (2026–2027)
    ↓ 6–12 months interoperability testing
Multi-vendor certified products (2027–2028)
    ↓ Operator procurement + deployment cycle
Live multi-vendor AI beam management at scale (2028–2029)
```

Compare this to energy optimization (already deployed) and beam management (vendor-proprietary today) — the standards pipeline is 4–5 years behind what Ericsson/Nokia are shipping.

---

## The 3GPP vs. O-RAN relationship (for o-ran-xapps.md)

A persistent source of confusion in the industry:

| Scope | 3GPP | O-RAN Alliance |
|-------|------|----------------|
| What it specifies | NR air interface, core network, management interfaces | RAN architecture (CU/DU split), open fronthaul, RIC, xApp/rApp interfaces |
| AI work | AI in the air interface (beamforming, CSI, positioning) | AI via xApps/rApps running on the RIC |
| Relationship | 3GPP specifies what the gNB does; O-RAN specifies how components connect | Complementary, not competing |
| Completion status | Rel-18 done; Rel-19 in progress | Architecture defined; production deployments limited |

**Key insight:** O-RAN xApps can run AI today using existing E2/A1 interfaces — this is why energy optimization and fault detection (which use high-level KPI data over O1/E2) are further along than beam management AI (which requires sub-ms PHY-layer decisions that O-RAN/E2 latency can't yet support reliably).

---

## Maturity tier mapping (from this timeline)

| Feature | Current standard status | Commercial status |
|---------|------------------------|-------------------|
| Energy optimization AI | No specific 3GPP spec needed — uses existing KPI counters | **Tier 1** — deployed |
| Predictive fault detection | No specific spec needed — uses OSS/KPI data | **Tier 2** — deployed at some operators |
| AI beam management (single-vendor) | Proprietary, ahead of specs | **Tier 2** — deployed in single-vendor networks |
| AI beam management (multi-vendor) | Rel-19 WI, spec ~2026 | **Tier 3** — 2027–2028 |
| O-RAN xApps (energy/fault) | O-RAN architecture defined | **Tier 2–3** — trial/early production |
| AI-native RAN (PHY-embedded) | Rel-20 study item | **Not ready** — 2029+ |
| Fully autonomous RAN | No spec exists | **Not ready** |

---

## What to extract for beam-management.md

- The Rel-18/19 timeline explains why single-vendor proprietary AI beam management is deployed today while multi-vendor standardized AI beam management is years away
- The "study item → work item → normative spec → product → interoperability" pipeline is the core insight for setting operator expectations
- The 2028–2029 multi-vendor timeline is the realistic horizon for operators with mixed-vendor RAN deployments
