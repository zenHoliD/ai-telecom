# RAN AI Project Failure Patterns — Synthesis

**Source:** Synthesis across multiple industry sources
**Primary inputs:** TM Forum "Autonomous Networks: Lessons from Early Deployments" (various years), Heavy Reading O-RAN deployment reports, IEEE academic papers on ML in network management, Analysys Mason Open RAN deployment challenges (April 2024)
**Supporting context:** Confirmed and extended by `ai-ran-reality-check-2026.md` and `oran-deployment-reality-2026.md` already in this study plan
**Quality tier:** [IND] Synthesis — treat as a structured hypothesis set, not primary evidence
**Relevant to:** `04-failure-modes.md`

---

## Why published failure data is sparse

Operators don't advertise what went wrong. Vendor case studies only document successes. Academic papers cover lab environments. This creates a publication bias problem: the industry literature overrepresents successful deployments.

The best sources for failure patterns are:
1. TM Forum "Lessons Learned" papers — operators share anonymized failure patterns in trust environments
2. Heavy Reading and Analysys Mason — analysts interview operators off-record; findings are paraphrased
3. The absence of follow-up case studies — if an operator published a trial press release but no production deployment announcement 18 months later, the trial stalled

**For this guide:** your insider operator experience is more valuable than any published source in this section. Use the framework below as a checklist to confirm, refute, or extend.

---

## Failure mode taxonomy

### FM-1: Data quality and counter inconsistency

**What happens:** The AI model is trained on KPI counter data, but telecom KPI counters were designed for human dashboards, not ML training. Common problems:
- Counter definitions vary by vendor (Ericsson "RRC Connected Users" ≠ Nokia "RRC Connected Users")
- Missing data windows during maintenance or upgrades — model learns from gaps as signal
- Clock skew between network elements corrupts time-series training data
- KPI resolution (15-minute intervals) too coarse for models requiring sub-minute inputs

**Signal that this is happening:** Model performs well in trials on vendor X's network, degrades on mixed-vendor deployments.

**Industry evidence:** Documented in TM Forum AN-phase2 reports and Heavy Reading O-RAN challenge surveys. The O-RAN deployment reality source notes that "technical complexity of multi-vendor integration" is a leading deployment barrier — data inconsistency is a significant component.

---

### FM-2: Trial-to-production gap

**What happens:** Pilot conditions are controlled in ways that live production isn't:
- Pilot sectors are chosen because they're stable (not representative of worst-case areas)
- Maintenance windows are coordinated with the pilot period
- Vendor PS (professional services) engineers are on-site during the trial
- Traffic load during the trial may not represent peak or seasonal variability

When the AI moves to production network-wide, it encounters edge cases the model has never seen.

**Signal:** Outstanding performance in pilot press release → 12–18 months of silence → no production announcement.

**Industry evidence:** This is the primary subtext of the AI-RAN reality check source: "No operator has announced commercial deployments despite trial activity." That's the gap.

---

### FM-3: Vendor proprietary data lock-in

**What happens:** The AI feature requires a real-time proprietary data stream (e.g., Ericsson's ENDC counters, Nokia's BBU-internal telemetry) that isn't exposed via standard interfaces (O1, E2). This creates two problems:
1. Third-party AI/xApp vendors can't access the data → single-vendor AI ecosystem
2. Operator who builds on proprietary stream can't migrate to a different vendor's RAN without losing the AI capability

**Signal:** Vendor's AI demo always shows their own RAN equipment feeding the model.

**Industry evidence:** The O-RAN deployment reality source documents the "interoperability gaps" between RAN vendors and O-RAN components. Much of this is data interface incompleteness. The AI-RAN reality check source's "architectural divergence" section between Ericsson and Nokia is a direct example.

---

### FM-4: xApp conflict (competing optimization loops)

**What happens:** Multiple AI optimization loops target different KPIs on the same cell or cluster:
- Energy optimization xApp wants to put a cell to sleep
- Load balancing xApp wants to hand off traffic to that cell
- Beam management xApp is adjusting the same antenna weights

Without a conflict resolution layer, the xApps oscillate or produce unpredictable interference patterns.

**Signal:** KPIs improve for one metric and degrade for another after multi-xApp deployment.

**Industry evidence:** Documented as an open architectural problem in the O-RAN RIC/xApp source. The O-RAN Alliance's WG2 (non-RT RIC) and WG3 (near-RT RIC) are working on conflict management frameworks, but no widely-deployed solution exists as of April 2026.

---

### FM-5: NOC trust and organizational adoption failure

**What happens:** The model works technically. The NOC team doesn't use it.
- First major incident after AI activation triggers manual override → AI is switched off
- Operators revert to "human-in-the-loop" (L2) permanently after a single false positive at L3
- Alerts are generated faster than NOC can process them → alert fatigue → alerts ignored

The AI-RAN reality check source identifies workforce readiness as "the primary bottleneck" — this is the mechanism.

**Signal:** AI is deployed, but "automation rate" (% of actions executed without human approval) stays at 0% after go-live.

**Industry evidence:** TM Forum autonomous networks surveys consistently find that L2→L3 transition (the closed-loop step) is blocked more by organizational readiness than technical capability. The O-RAN deployment reality source confirms "workforce and skill gaps" as a leading barrier.

---

### FM-6: Baseline measurement impossibility

**What happens:** After deployment, the operator cannot prove the AI caused the improvement. Possible alternative explanations for any KPI improvement:
- Simultaneous software upgrade with performance benefits
- Traffic growth patterns that match the AI's apparent effect
- Seasonal variation (energy savings in summer look bigger if baseline was winter)
- The Hawthorne effect — network engineers pay more attention during pilot periods

Without a clean A/B test, CFOs and CTOs are skeptical of the ROI numbers. The project "succeeds" technically but doesn't generate the budget approval needed for scale-up.

**Signal:** Vendor presents A/A comparison ("site with AI vs. site without AI on same day") but no extended longitudinal study.

**Industry evidence:** McKinsey's research on AI ROI consistently identifies the baseline problem as the reason AI "works in labs but struggles to justify in CFO reviews." This is also why Tier-1 classification in this guide requires operator case studies with documented baseline methodology.

---

### FM-7: Model drift

**What happens:** The model is trained on historical traffic data. Traffic patterns change:
- New residential development near cell → traffic profile shifts
- Major event venue nearby creates unexpected load spikes
- 5G migration changes traffic distribution between cells
- Seasonal patterns (e.g., summer tourism) create out-of-distribution conditions

Model performance degrades silently — there's no alarm that says "model is now making worse decisions."

**Signal:** KPI improvements that were stable for 6 months start eroding without any network changes.

**Industry evidence:** Less documented publicly, but well-understood in ML operations literature. The NVIDIA survey notes "configuration drift correction" as a top use case — model drift is the mirror problem (the network drifts away from what the model was trained on).

---

### FM-8: Integration complexity underestimated

**What happens:** The AI model is built and validated. Connecting it to the live network via OSS/BSS takes 2–3x as long as the model development.
- O1 interface to collect KPIs: vendor implementation variations → integration effort per vendor
- A1 interface to push policy updates: latency and reliability requirements not met in initial implementations
- E2 interface (near-RT RIC): most demanding, fewest tested implementations in production

**Signal:** Trial phase is announced, integration phase is never mentioned in follow-up press releases.

**Industry evidence:** The O-RAN deployment reality source quantifies this directly: full E2E O-RAN stack deployment takes 12–18+ months. The AI model is rarely the longest pole.

---

## Failure mode severity matrix

For `04-failure-modes.md`, use this to prioritize which modes to document in depth:

| Failure mode | Frequency | Solvability | Time to discover |
|--------------|-----------|------------|-----------------|
| FM-1: Data quality | Very high | Moderate (process fix) | Early (testing) |
| FM-2: Trial-to-production gap | High | Low (fundamental) | Late (post-trial) |
| FM-3: Vendor lock-in | High | Low (ecosystem constraint) | Late (scale-up) |
| FM-4: xApp conflict | Medium | Moderate (architecture fix) | Medium (multi-xApp) |
| FM-5: NOC adoption | High | Moderate (change management) | Medium (post-deploy) |
| FM-6: Baseline measurement | Medium | Low (structural) | Late (ROI review) |
| FM-7: Model drift | Medium | Moderate (MLOps practice) | Late (months post-deploy) |
| FM-8: Integration complexity | High | Moderate (tooling/standards) | Medium (deployment) |

**Highest priority for the guide:** FM-2, FM-3, and FM-5 — these are the most frequent, least discussed publicly, and most relevant to an operator audience evaluating whether to commit to RAN AI.
