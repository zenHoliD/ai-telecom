# Ericsson & Vodafone UK — AI 5G Energy Efficiency Trial

**Source:** Ericsson Press Release / Vodafone UK Newscentre (2025)
**URL:** https://www.ericsson.com/en/press-releases/3/2025/vodafone-uk-and-ericsson-trial-ai-solutions-for-improved-5g-energy-efficiency
**Quality tier:** [OP] Operator case study
**Relevant to:** `02-use-cases/energy-optimization.md`

---

## Summary

Vodafone UK and Ericsson reduced daily power consumption of 5G Radio Units by **up to 33%** at select sites across London. The 5G Deep Sleep feature specifically achieved **up to 70% energy savings** during low-traffic periods.

Earlier Vodafone deployment (referenced in RCR Wireless, Oct 2024) achieved **14% savings in energy consumption per site** using AI sleep mode across MIMO clusters — this is the more conservative, network-wide production figure. The 33% figure is from a more recent focused trial.

---

## Solution: Ericsson Service Continuity AI App Suite

**Component:** Intelligent Energy Efficiency — dynamically manages RAN power consumption based on live traffic data using AI/ML.

### Three use cases deployed

**1. 5G Deep Sleep**
- AI-powered predictive algorithms allow radios to enter ultra-low energy hibernation states
- Activates rapidly when traffic increases
- Up to 70% energy savings during low traffic periods

**2. 4G Cell Sleep Mode Orchestration**
- Creates behavioral models of network cells
- Optimizes sleep parameters while balancing energy savings and performance
- Manages sleep across MIMO clusters autonomously

**3. Radio Power Efficiency Heatmap**
- ML tool that visualizes all network cells
- Identifies underperforming sites for targeted optimization

---

## Key details for your analysis

- **Deployment model:** AI on RAN (external AI instructing RAN to sleep; not embedded in baseband)
- **Closed-loop or open-loop:** Closed-loop — AI acts autonomously based on live traffic, no human approval step
- **Baseline:** Not specified in press release — a weakness in this source
- **Impact on service quality:** Vodafone CNO confirmed "significantly improved energy efficiency without impacting the service"
- **Geography:** London trial sites; other Vodafone UK markets implied

---

## Other operators cited (from RCR Wireless Oct 2024)

| Operator | Savings | Solution |
|----------|---------|----------|
| Vodafone | 14% per site | Ericsson AI sleep mode (MIMO clusters) |
| Umniah Jordan | 20% daily | Ericsson Intelligent RAN Power Saving |
| Three UK | Up to 70% at select sites | AI-powered hardware + software |
| Far EasTone (Taiwan) | 25% daily RAN energy | Ericsson Service Continuity AI App |

---

## Watch-outs

- "Up to 70%" figures are peak savings at optimal conditions, not average network-wide
- 14% is the more conservative, replicable network-wide production number
- All figures are Ericsson solutions — Nokia and Huawei claim similar but with different architectures
- Press releases don't disclose baseline traffic load, site density, or frequency bands
