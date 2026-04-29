# xApp Conflict Management in O-RAN — Research State

**Source:** IEEE Communications Magazine, "Conflict Management for xApps in O-RAN" (2024); O-RAN Alliance WG3 Technical Report on xApp Conflict Mitigation (2024); IEEE Access, "Multi-Agent Reinforcement Learning for O-RAN xApp Coordination" (2025)
**Quality tier:** [IND] Academic research + standards body — describes the problem accurately, not yet a solved production pattern
**Relevant to:** `02-use-cases/o-ran-xapps.md`, `04-failure-modes.md` (FM-4)

---

## Why this matters

FM-4 (xApp conflict) is the structural architectural risk in O-RAN's AI vision. As operators move from single-xApp deployments to multi-xApp environments, this becomes the dominant technical problem. Understanding the current state of research is required to accurately describe the maturity of O-RAN xApps.

---

## The conflict taxonomy

O-RAN research identifies three types of xApp conflict:

### 1. Direct conflict (resource-level)
Two xApps attempt to control the same RAN parameter simultaneously.
- Energy xApp sets cell power to minimum; coverage xApp sets it to maximum
- Scheduling xApp changes PRB allocation; QoS xApp changes the same allocation 50ms later

**Impact:** Oscillation. KPIs are worse than with no AI at all because the network is unstable.

### 2. Indirect conflict (objective-level)
Two xApps pursue compatible resource actions but conflicting optimization objectives.
- Load balancing xApp shifts users to a cell to improve utilization
- Interference management xApp reduces that cell's power to protect a neighbor cell
- Net result: users are on a cell with degraded signal quality

**Impact:** No single KPI alarm, but overall network quality degrades. Difficult to diagnose.

### 3. Temporal conflict
One xApp acts, then another acts on the (now changed) state, creating a feedback loop.
- Handover xApp triggers a mass handover based on traffic conditions
- Beam management xApp responds to the new traffic distribution by adjusting weights
- Handover xApp sees changed signal conditions and triggers another handover cycle

**Impact:** Periodic instability correlated with xApp control loop timing. Classic control theory instability.

---

## Current solutions in the research literature

### Conflict detection layer (O-RAN Alliance approach)
The O-RAN Alliance WG3 proposes a conflict mitigation module inside the Near-RT RIC:
- xApps register their intended actions before executing
- Conflict detection logic checks against registered scope of other active xApps
- Conflicting action is either blocked, delayed, or arbitrated

**Status:** Specified in O-RAN WG3 technical reports. **Not yet widely implemented in production RIC platforms.**

Known limitations of this approach:
- Requires xApps to accurately declare their scope at registration (self-reporting problem)
- Doesn't handle indirect conflicts or temporal conflicts (only direct resource overlap)
- Adds latency to the control loop — a problem for near-RT xApps operating in the 10–1000ms window

### Domain isolation (Rakuten's operational approach)
Manually define non-overlapping resource domains for each xApp:
- Energy xApp: only controls cells below 20% load threshold
- Mobility xApp: only modifies handover margins, not power or scheduling
- Interference xApp: only controls cells near cell edge (RSRP < threshold)

**Status:** In production at Rakuten. Works, but requires manual governance overhead and limits xApp generality.

**Key limitation:** As the number of deployed xApps grows, the domain isolation becomes increasingly complex to maintain. Works for 3–5 xApps; unclear how it scales to 15+.

### Multi-agent reinforcement learning (MARL) — research frontier
Train xApps as cooperative MARL agents that learn to coordinate without explicit conflict rules.

**Status:** Promising in simulation (IEEE papers show 15–30% improvement over non-cooperative xApps). **Not in production anywhere as of 2025.** Requires:
- Shared state representation across xApps (data sharing problem)
- Joint training infrastructure
- Retraining when any single xApp is updated

---

## What this means for o-ran-xapps.md

**Honest maturity statement:** The xApp conflict problem is not solved. Current production deployments (including Rakuten) use administrative domain separation, not technical conflict resolution. This is not a flaw in the architecture — it's a practical engineering approach — but it limits the number and generality of xApps that can be deployed simultaneously.

**Implication for operators considering xApp deployment:**
- First xApp: straightforward, conflict is not an issue
- Second and third xApp: domain isolation is manageable with clear documentation
- Beyond three xApps: requires dedicated architectural governance, not just engineering effort

**The 2027–2028 horizon for multi-xApp production is realistic:** that's when standardized conflict mitigation modules and mature RIC platforms are expected to support 5+ concurrent xApps without manual domain governance.

---

## Implication for failure-modes.md (FM-4)

FM-4's severity is **medium frequency, medium-high impact**. It doesn't affect operators with a single xApp deployment (most current production deployments). It becomes the primary technical risk when an operator tries to run 3+ optimization loops simultaneously — which is the promised value of O-RAN. The failure mode is most likely to manifest during scale-up, not initial deployment.
