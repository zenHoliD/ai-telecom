# 01 — AI in RAN: Landscape Map

*Last updated: April 2026*

---

## Vocabulary

Before the map, the terms. These get used inconsistently — knowing the
precise definitions helps you filter what vendors are actually saying.

| Term | What it means |
|------|--------------|
| **ML / Machine Learning** | The actual technology behind most "AI in RAN" — models trained on data to predict or optimize. Not ChatGPT-style AI. Mostly supervised learning, reinforcement learning, and anomaly detection. |
| **Inference** | Running a trained ML model to produce an output (e.g., "reduce power on this cell"). Happens in real time or near-real time in the network. |
| **Training** | Building the model from historical data. Usually happens offline, not on the live network. |
| **SON (Self-Organizing Network)** | The predecessor to AI in RAN. Rule-based automation for coverage, capacity, and interference (CCO, MRO, MLB). Still widely deployed. AI is now extending what SON can do. |
| **RIC (RAN Intelligent Controller)** | The O-RAN component that hosts AI/ML apps. Near-RT RIC runs xApps (10ms–1s loops). Non-RT RIC runs rApps (>1s, policy-level). |
| **xApp** | An application running on the Near-RT RIC. Handles fast, local decisions — interference, scheduling hints, beam steering. |
| **rApp** | An application running on the Non-RT RIC. Handles slower, global optimization — energy policy, capacity planning, model training. |
| **O-RAN** | Open RAN architecture defined by the O-RAN Alliance. Separates RAN hardware from software via open interfaces. Enables third-party AI apps via the RIC. |
| **AI-native RAN** | RAN where AI/ML is embedded directly in the RAN algorithms and functions — not a separate component. 3GPP Rel-18/19 work item. |
| **CSI (Channel State Information)** | Data describing radio channel conditions. One of the first AI targets in 3GPP — compressing and predicting CSI with ML (TR 38.843). |
| **Closed-loop automation** | The system acts on its own inference without human approval. Open-loop = inference generates a recommendation; a human or rule-based system acts on it. |

---

## The Three Compute Models

Not all "AI in RAN" is the same. The most important distinction is where
the compute lives and how tightly AI is coupled to the RAN stack.

### AI on RAN

**What it is:** AI runs on a separate system. The RAN is still a black box —
you're reading its outputs (KPIs, alarms, counters) but not touching its
internals. AI sits in the OSS, a cloud analytics platform, or a vendor's
management system.

**Hardware dependency:** None. Works on any RAN, including traditional
single-vendor proprietary hardware. This is why it's the easiest to deploy.

**What it does today:**
- Energy optimization: predict low-traffic periods, instruct the RAN to
  put carriers or cells to sleep. The RAN executes; the AI decides when.
- Predictive fault detection: spot anomalies in KPI streams before an
  outage. Alert the NOC earlier than threshold-based alarms.
- Capacity planning: model traffic growth, recommend sites and frequencies.

**The catch:** You can optimize around the RAN, but you can't change what
the RAN does internally. There's a ceiling on how much you can improve
without touching the stack.

---

### AI+RAN

**What it is:** RAN is cloudified — running as software on commercial
off-the-shelf (COTS) servers instead of dedicated proprietary hardware.
Because it's now software on shared compute, you can run AI workloads on
the same physical or virtual infrastructure as the RAN software.

**Hardware dependency:** Requires cloudified RAN (vRAN or Cloud RAN). You
can't do this on a traditional black-box base station. This is the gate that
limits adoption — most operators are still partially or fully on legacy RAN.

**What it enables:**
- AI inference co-located with RAN processing. Lower latency than external
  analytics (no round-trip to a separate system).
- GPU-accelerated RAN (NVIDIA's approach): run massive MIMO signal processing
  and AI inference on the same GPU cluster.
- Vendor-integrated AI features that wouldn't be possible on locked hardware.

**Reality check:** The cloudification prerequisite makes this slower to
reach at scale. Operators with significant vRAN/Cloud RAN footprint
(Rakuten, Dish/EchoStar, some Tier-1s in specific markets) are further
along. Most operators are in a mixed state — some vRAN pockets, mostly legacy.

---

### AI in RAN

**What it is:** AI/ML is embedded inside the RAN algorithms themselves.
Not a separate component or co-located workload — the scheduler, beam
management function, or channel estimation algorithm has ML models baked
into it. The RAN *is* AI-native.

**Hardware dependency:** Requires either cloudified RAN or new AI-capable
radio hardware. Also requires 3GPP standardization for interoperability.

**Where it stands (3GPP):**
- **Release 18 (2024):** Study item TR 38.843 — evaluated AI/ML for three
  use cases: CSI feedback compression, beam management, and positioning.
  Proved feasibility, identified protocol gaps.
- **Release 19 (2025-2026):** "CSI prediction" promoted to Work Item.
  Target prediction horizons: 4ms and 8ms. "Two-sided model" (UE and base
  station both run ML) also in progress.
- **Release 20+ / 6G:** Full AI-native RAN — AI embedded across the stack,
  not just selected functions.

**In vendor products today:** Proprietary, not standardized. Ericsson, Nokia,
and Huawei all have ML inside their baseband software (beam management,
interference, scheduling). But it's single-vendor, not interoperable. The
3GPP work is what will make it open and multi-vendor.

**The catch:** This is where the most transformative potential is — and
where you're furthest from deploying it in a multi-vendor, operator-
controlled way. Proprietary AI-in-RAN exists today but is a black box
inside a black box.

---

### "AI for RAN" — the marketing term

You'll see this used by vendors as a product line or portfolio name. It has
no precise technical meaning — it just means "AI that does something useful
for RAN," which covers all three models above.

**When you see it:** Ask which of the three models it actually is. If the
vendor can't answer that, they haven't committed to a specific architecture
yet (or the product doesn't exist yet).

---

## What's Real in Each Model Today

| Model | Deployed & working | In trials | Not ready |
|-------|-------------------|-----------|-----------|
| **AI on RAN** | Energy optimization (7-30% savings), predictive fault detection, capacity planning | GenAI for NOC ticket summarization | GenAI for real-time decisions |
| **AI+RAN** | GPU-accelerated vRAN (NVIDIA + select operators), AI inference on cloud RAN | Co-located xApp inference on cloud RAN | Full closed-loop on cloud RAN at scale |
| **AI in RAN** | Proprietary vendor ML (single-vendor, closed) | O-RAN xApps on Near-RT RIC (~13% operators in production O-RAN) | Standardized interoperable AI-native RAN (Rel-19+ work items) |

---

## What's Driving the Roadmap

Two parallel tracks define where AI in RAN goes:

**Track 1 — O-RAN Alliance (architecture)**
Open interfaces that let third-party AI apps (xApps, rApps) plug into any
vendor's RAN via the RIC. The vision: operators own the AI layer, not just
the vendor. The reality: multi-vendor integration is still complex, only
~13% of operators have production O-RAN, and xApp conflict management
(what happens when two xApps give contradictory instructions) is an unsolved
problem.

**Track 2 — 3GPP (standardization)**
Defining how AI/ML functions are specified inside RAN nodes for
interoperability. Release 18 studied it. Release 19 is formalizing the
first use cases. Full AI-native RAN is Release 20+/6G. Timeline: anything
standardized in Rel-19 won't be in commercial products at scale until
2027-2028 at the earliest.

---

## How to Read a Vendor Claim

Five questions. If a vendor can't answer all five, the claim is not evidence.

1. **Which operator deployed this in production?**
   "Operator X" is evidence. "A Tier-1 operator in Europe" is a sign it's
   under NDA or the results weren't strong enough to publish.

2. **What was the baseline?**
   A 20% improvement from what starting point, under what traffic conditions?
   Results in a lightly-loaded lab cell don't replicate in a dense urban cluster.

3. **Was it closed-loop or open-loop?**
   Open-loop (recommendation + human approval) is easier to deploy and gets
   inflated in demos. Closed-loop (AI acts automatically) is what scales —
   and is harder to validate.

4. **What data did it require?**
   If it needed new probes, new interfaces, or proprietary data streams,
   the deployment cost is higher than the demo suggests.

5. **What happened after the trial?**
   Pilots succeed. Production is where things break. Ask if the operator
   moved to production and how long it's been running.

---

## Where to Focus in 2026

**High signal — worth your time now:**

- **Energy optimization (AI on RAN):** The one area with clean operator
  evidence, fast deployment, and clear ROI. Vodafone 14% savings/site is a
  real reference (Ericsson case study, 2023-2024). Multiple operators in the
  7-30% range. If your org hasn't evaluated this, it's leaving money on the
  table.

- **O-RAN + xApps architecture:** Not because you should deploy xApps today,
  but because this is where the industry is building toward. Understanding
  the Near-RT RIC / Non-RT RIC split, xApp vs. rApp scope, and the open
  interface specs (E2, A1, O1) now means you'll be fluent when it matters.

- **3GPP Rel-18/19 AI work items:** TR 38.843 is publicly available.
  Understanding what 3GPP is and isn't standardizing tells you which vendor
  claims are ahead of the standards (marketing) and which are aligned
  (roadmap).

**Low signal — safe to ignore until 2027-2028:**

- **AI-native RAN at the edge:** MWC 2026 demos and stadium pilots.
  Products at scale are 2-3 years out.

- **Fully autonomous closed-loop RAN:** Requires solving xApp conflict
  management, regulatory approval, and operator trust that isn't there yet.

- **GenAI for real-time network management:** Latency and reliability
  constraints of RAN are incompatible with current LLM inference speeds.
  Useful for NOC tooling (ticket summarization, runbook generation) — not
  for anything in the data plane.

---

## Sources

See [`sources.md`](sources.md) for quality-rated references.

Key references used here:
- 3GPP TR 38.843 — AI/ML for NR air interface (Rel-18 study item)
- O-RAN Alliance Architecture Specification
- NVIDIA State of AI in Telco Survey 2026
- Ericsson AI-powered RAN energy efficiency case study (Vodafone, 2023-2024)
- RCR Wireless: "How will AI drive 5G RAN energy efficiency?" (Oct 2024)
- IEEE ComSoc: "Overview of AI in 3GPP RAN Release 18"

---

*Living document. Update after MWC, ONS, 3GPP plenaries, or when a new
operator case study with real numbers is published.*
