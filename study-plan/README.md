# Study Plan — AI in RAN Guide

Reading material organized by the file each source prepares you to write.
Quality tiers: **[OP]** operator case study · **[IND]** independent analyst · **[STD]** standards body · **[VND]** vendor

---

## Source files (actual content)

| File | Topic | Tier | Prepares |
|------|-------|------|----------|
| [sources/ericsson-vodafone-energy-trial.md](sources/ericsson-vodafone-energy-trial.md) | Vodafone UK / Ericsson 5G energy trial — 14–33% savings | [OP] | energy-optimization |
| [sources/rcr-wireless-ai-ran-energy.md](sources/rcr-wireless-ai-ran-energy.md) | 4 operator energy deployments, AI maturity stages | [IND] | energy-optimization |
| [sources/nvidia-state-ai-telecom-2026.md](sources/nvidia-state-ai-telecom-2026.md) | NVIDIA survey — 1,000+ telcos, ROI use cases, adoption | [IND] | predictive-fault, roi-framework |
| [sources/noc-ai-predictive-fault.md](sources/noc-ai-predictive-fault.md) | NOC AI, MTTR reduction, predictive vs reactive detection | [IND] | predictive-fault |
| [sources/oran-ric-xapps-rapps.md](sources/oran-ric-xapps-rapps.md) | Near-RT / Non-RT RIC, xApp/rApp architecture, interfaces | [IND] | o-ran-xapps |
| [sources/oran-deployment-reality-2026.md](sources/oran-deployment-reality-2026.md) | Real O-RAN numbers: AT&T, DT, Vodafone, Airtel | [IND] | o-ran-xapps |
| [sources/ieee-comsoc-ai-3gpp-rel18.md](sources/ieee-comsoc-ai-3gpp-rel18.md) | 3GPP Rel-18 AI/ML: CSI, beam management, positioning | [IND] | beam-management, o-ran-xapps |
| [sources/ai-ran-reality-check-2026.md](sources/ai-ran-reality-check-2026.md) | AI-RAN hype analysis — no production deployments, standards gap | [IND] | failure-modes, landscape |

---

## Reading order (recommended)

1. `sources/ericsson-vodafone-energy-trial.md` + `sources/rcr-wireless-ai-ran-energy.md` → write `energy-optimization.md`
2. `sources/ieee-comsoc-ai-3gpp-rel18.md` → write `beam-management.md`
3. `sources/noc-ai-predictive-fault.md` + `sources/nvidia-state-ai-telecom-2026.md` → write `predictive-fault.md`
4. `sources/oran-ric-xapps-rapps.md` + `sources/oran-deployment-reality-2026.md` → write `o-ran-xapps.md`
5. All of the above → write `roi-framework.md`
6. `sources/ai-ran-reality-check-2026.md` + your insider experience → write `failure-modes.md`

---

## Still needed (search manually)

- **[STD]** 3GPP TR 38.843 full text — free at 3gpp.org (search "TR 38.843")
- **[STD]** O-RAN Alliance WG1/WG2/WG3 specs — free at o-ran.org (registration required)
- **[STD]** TM Forum IG1230 Autonomous Networks — free at tmforum.org
- **[IND]** Analysys Mason Open RAN report — paid; check if org has access
- **[IND]** Gartner 2023 MTTR reduction benchmark — paywalled; the 40% figure needs primary source verification
