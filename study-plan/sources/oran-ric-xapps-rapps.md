# O-RAN: RIC, xApps, and rApps Architecture

**Source:** RIMEDO Labs — RAN Intelligent Controller Overview (2023–2024)
**URL:** https://rimedolabs.com/blog/ran-intelligent-controller-ric-overview-xapps-and-rapps/
**Quality tier:** [IND] Independent technical blog (O-RAN Alliance member)
**Relevant to:** `02-use-cases/o-ran-xapps.md`, `01-landscape.md` reference

---

## The RIC split

The RAN Intelligent Controller abstracts functions traditionally locked inside the base station (eNB/gNB) into applications that run on open platforms.

| Component | Timescale | Interface | Role |
|-----------|-----------|-----------|------|
| Non-RT RIC | > 1 second | A1 | Policy creation, AI/ML model training, long-horizon optimization |
| Near-RT RIC | 10ms – 1s | E2 | Near real-time control of RAN elements |
| SMO | N/A | O1 | Orchestration, OAM, non-RT RIC host |

---

## xApps (Near-RT RIC)

**Definition:** Applications that run on the Near-RT RIC and directly control RAN functions through the E2 interface.

**Timescale:** > 10ms and < 1s control loops

**What xApps do:**
- Handover optimization
- Radio link monitoring
- Mobility management
- Load balancing
- Traffic steering
- Interference management
- Slicing policy updates
- Beam steering

**How they work:** xApps subscribe to E2 service models (E2SMs) that define what data they can read from RAN elements and what control actions they can send. An xApp can issue a **RIC Control** message (direct action) or a **RIC Policy** message (set a parameter boundary the RAN respects).

---

## rApps (Non-RT RIC)

**Definition:** Modular applications on the Non-RT RIC that provide value-added services through policy and enrichment information — they don't directly control RAN functions.

**Timescale:** > 1 second (typically minutes to hours)

**What rApps do:**
- AI/ML model training and deployment
- Long-horizon energy optimization policies
- Capacity planning
- Configuration management
- Enrichment information to Near-RT RIC via A1

**Key distinction:** rApps influence the network indirectly — they set policies that xApps or RAN elements act on. xApps act directly.

---

## Key interfaces

**A1 (Non-RT RIC → Near-RT RIC):**
- Intent-based interface
- Carries: policies, enrichment information, ML model management
- Direction: top-down (SMO/Non-RT RIC pushes to Near-RT RIC)

**E2 (Near-RT RIC ↔ RAN elements):**
- RAN closed-loop interface
- Carries: RIC control messages, RIC policy, RAN data reports
- Bidirectional: Near-RT RIC controls E2 Nodes; E2 Nodes report back

**O1 (SMO ↔ RAN elements):**
- Traditional OAM interface (configuration, fault, performance management)
- Not a real-time control interface

---

## The unsolved problem: xApp conflict management

When two xApps send contradictory instructions to the same RAN element (e.g., energy xApp wants to shut down a carrier; capacity xApp wants to activate it), there is no standardized resolution mechanism. The O-RAN Alliance is working on conflict mitigation frameworks, but this remains an open problem in production deployments.

---

## Production reality check

~13% of operators are in production O-RAN (source: industry estimates, April 2026). The integration complexity — multi-vendor testing, E2SM standardization gaps, conflict management — explains the gap between architectural ambition and deployment reality.
