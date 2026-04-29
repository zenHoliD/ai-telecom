# Rakuten Mobile / Rakuten Symphony — Production O-RAN with xApps

**Source:** Rakuten Symphony technical documentation, Mobile World Live interviews (2024–2025), Heavy Reading "Rakuten: Lessons from the world's first cloud-native MNO" (2025)
**Supporting:** Rakuten Symphony website, IEEE VTC 2024 paper on Rakuten RIC deployment
**Quality tier:** [OP] Operator case study (primary), [IND] analyst analysis (secondary)
**Relevant to:** `02-use-cases/o-ran-xapps.md`, `04-failure-modes.md`

---

## Why Rakuten matters

Rakuten Mobile is the most cited production O-RAN reference in the industry for two reasons:
1. It is the only tier-1 operator that built a greenfield fully virtualized, cloud-native network from scratch
2. It runs a production Near-RT RIC with active xApps — not a trial, not a lab

Every other "production O-RAN" operator is deploying open-interface hardware on a legacy core, or running a RIC alongside (not instead of) traditional RAN management. Rakuten eliminated the legacy layer entirely.

---

## What's actually in production (as of 2025)

| Component | Status | Detail |
|-----------|--------|--------|
| Near-RT RIC | Production | Running on Kubernetes, supports multiple xApps concurrently |
| Non-RT RIC / SMO | Production | Handles A1 policy, model management, offline training |
| Energy optimization xApp | Production | Sleep mode management; documented savings |
| Coverage / mobility xApp | Production | Handover parameter optimization across cells |
| Traffic steering xApp | Production | Load balancing between cells and layers |
| QoS management xApp | Trial | Packet-level optimization; not fully autonomous yet |

---

## Architecture decisions that differ from the O-RAN Alliance blueprint

Rakuten's actual deployment diverges from O-RAN Alliance spec in important ways:

1. **Single-vendor RU/DU/CU stack** — Rakuten used NEC and Altiostar (now Mavenir) for software, with Nokia/NEC radios. It's not truly multi-vendor at the RAN stack level. The openness is at the management layer.

2. **Custom E2 service models** — The standard E2SMs (for KPM, RC, and NI) were not complete when Rakuten deployed. They implemented proprietary E2SMs and are migrating to standards over time.

3. **Conflict resolution via domain separation** — Rather than a technical conflict arbitration layer, Rakuten limits xApp scope: each xApp owns a defined resource domain (e.g., energy xApp only touches cells below 20% load; mobility xApp only acts on handover margins). This prevents FM-4 (xApp conflict) through administrative control, not technical mediation.

---

## Documented challenges

### Cost of operations at scale
Rakuten's OPEX has been higher than expected. Cloud-native RAN requires:
- Kubernetes expertise at the NOC
- Continuous integration pipelines for xApp updates
- On-call for both RAN and IT infrastructure

The initial promise of OPEX reduction from automation has been partially offset by the new skill requirements. Rakuten Symphony is now selling this operational model to other operators — which means they've industrialized it, but it requires significant upfront investment.

### xApp development lifecycle
Building a new xApp takes 4–6 months from concept to production for Rakuten's in-house team. This includes:
- E2 interface integration and testing (often 50%+ of total time)
- RIC platform certification
- Conflict domain validation (ensuring xApp doesn't overlap with others)
- A/B testing in trial cells before production rollout

This is the reality behind "you can deploy a new optimization in weeks" — that's true for a pre-built xApp from a marketplace. A custom xApp is months.

### Network performance vs. traditional RAN
Rakuten's 5G performance metrics are competitive but not definitively superior to Docomo or SoftBank (traditional RAN operators). The advantage is operational flexibility, not inherent radio performance.

---

## What Rakuten proves

1. **xApps in production are technically feasible** — the architecture works at operator scale
2. **Multi-xApp conflict management requires explicit domain governance** — technical arbitration is not yet the solution
3. **E2 interface development is the long pole** — not the AI model, not the RIC platform
4. **The ROI story is operational flexibility, not immediate OPEX reduction** — a 3–5 year horizon for cost benefits

---

## What Rakuten doesn't prove

- That traditional tier-1 operators (with 20,000+ existing sites on legacy RAN) can replicate this — Rakuten is a greenfield deployment
- That the O-RAN Alliance's multi-vendor vision works in practice at this scale — Rakuten is largely single-vendor per layer
- That current O-RAN standards (E2SMs, A1 policies) are sufficient — Rakuten uses custom extensions
