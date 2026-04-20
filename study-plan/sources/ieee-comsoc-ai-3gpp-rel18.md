# IEEE ComSoc — Overview of AI in 3GPP RAN Release 18

**Source:** IEEE Communications Society Technology News (2024)
**URL:** https://www.comsoc.org/publications/ctn/overview-ai-3gpps-ran-release-18-enhancing-next-generation-connectivity
**Secondary:** https://www.comsoc.org/publications/ctn/artificial-intelligence-3gpp-5g-advanced-survey
**Quality tier:** [IND] Independent standards body publication
**Relevant to:** `02-use-cases/beam-management.md`, `02-use-cases/o-ran-xapps.md`

---

## What Release 18 actually did

Release 18 (finalized 2024) is the **first 3GPP release to integrate AI/ML directly into the NR air interface**. Prior to Rel-18, AI in 3GPP was limited to core network analytics (NWDAF, introduced in Rel-15/17).

**Three primary use cases studied in TR 38.843:**

| Use case | Focus | Status in Rel-18 |
|----------|-------|-----------------|
| CSI feedback enhancement | Compression + prediction using two-sided models | Study item → WI in Rel-19 |
| Beam management | Reducing overhead, improving beam selection accuracy | Study item; Rel-19 normative work ongoing |
| Positioning accuracy | Non-line-of-sight environments | Study item |

---

## AI/ML framework defined in Rel-18

Five logical functions established:
1. **Data Collection** — what data flows into training and inference
2. **Model Training** — offline or online model building
3. **Model Management** — lifecycle: activation, switching, fallback, updates
4. **Inference** — running the model in real time
5. **Model Storage** — where models live in the network

**Lifecycle management approaches:**
- Functionality-based: features activate/deactivate as a unit
- Model identity-based: specific model versions tracked across UE and network

---

## CSI feedback (the furthest along)

- Rel-18 proved: two-sided model (encoder at UE, decoder at gNB) reduces CSI feedback overhead
- Time-domain prediction: single-sided model addresses channel aging in high-mobility scenarios
- Rel-19 promoted "one-sided CSI prediction" to a **Work Item** with target horizons of **4ms and 8ms**
- "Two-sided model" (UE and gNB both run ML) also normative work in Rel-19

---

## Beam management (relevant to your use case file)

- Objective: reduce beam search overhead, minimize latency, improve beam selection accuracy
- Approach: spatial-domain downlink beam prediction using historical channel data
- Rel-18 study proved feasibility and identified protocol gaps
- Rel-19 advancing normative specifications

**Key gap for beam management:** Two-sided models require coordinated inference between UE and gNB — complicating interoperability testing across vendors. This is why proprietary vendor implementations exist today but standardized multi-vendor beam management AI is 2027+ at earliest.

---

## Reliability requirement

A fundamental requirement across all Rel-18 AI use cases:
> "AI/ML-enabled features must remain at a level equal to or better than legacy non-AI/ML-based operations."

This is why all deployments require fallback mechanisms — the system must automatically revert to traditional algorithms if the AI model performance degrades.

Three reliability strategies:
1. **Model generalization** — one model that handles diverse scenarios
2. **Model switching** — swap between models for different conditions
3. **Model update / fine-tuning** — periodic updates for scenario drift

---

## What Rel-18 did NOT do

- Did not define interoperable multi-vendor AI beam management
- Did not standardize xApps or O-RAN RIC (that's O-RAN Alliance, not 3GPP)
- Did not enable AI-native scheduling or AI-embedded core RAN functions
- Did not produce deployable specifications — Rel-18 AI work items are mostly study items that feed Rel-19+ Work Items

---

## Timeline reality check

> "Anything standardized in Rel-19 won't be in commercial multi-vendor products at scale until 2027–2028 at the earliest."

Rel-19 closes in 2025–2026 → vendor implementation → interoperability testing → commercial availability = 2027–2028 minimum.

Proprietary vendor implementations (Ericsson, Nokia, Huawei) already exist today but are single-vendor and not based on Rel-18/19 specs — they are ahead of, but not aligned with, the standards.
