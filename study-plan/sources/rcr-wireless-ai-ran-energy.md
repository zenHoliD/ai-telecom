# RCR Wireless — How Will AI Drive 5G RAN Energy Efficiency?

**Source:** RCR Wireless News (October 14, 2024)
**URL:** https://www.rcrwireless.com/20241014/5g/how-will-ai-drive-5g-ran-energy-efficiency
**Quality tier:** [IND] Independent trade publication
**Relevant to:** `02-use-cases/energy-optimization.md`

---

## Industry context

- Energy costs represent **up to 10% of total opex** for telecom operators
- RAN accounts for **87% of operator energy consumption** (GSMA Intelligence)
- 5G NR standards are more energy-efficient per bit, but overall consumption is higher due to denser deployments and more antennas

---

## Confirmed operator deployments

| Operator | Country | Savings | Solution |
|----------|---------|---------|----------|
| Vodafone | UK | 14% per site | Ericsson AI sleep mode across MIMO clusters |
| Umniah | Jordan | 20% daily | Ericsson Intelligent RAN Power Saving |
| Three UK | UK | Up to 70% at select sites | AI-powered hardware + software |
| Far EasTone | Taiwan | 25% daily RAN energy | Ericsson Service Continuity AI App |

---

## How the AI works (Vodafone example)

AI algorithms analyze baseband traffic patterns and compare projected versus actual **Physical Resource Block (PRB) utilization**. Based on that comparison, the system autonomously decides whether to:
- Power down antenna branches
- Power down amplifiers
- Put entire carriers to sleep

This is a **closed-loop** deployment — no human approval step once thresholds are set.

---

## AI maturity journey (four stages)

1. **Rules-based automation** — static schedules (e.g., sleep at 2am, wake at 6am)
2. **Autonomous features** — AI adapts to unforeseen conditions dynamically
3. **Cognitive intent-driven networking** — automated decision-making with intent policies
4. **Zero-touch networking** — fully automated lifecycle management

Most production deployments today are at Stage 2. Stage 3–4 are in trials or on roadmaps.

---

## Market outlook

- AI RAN solutions projected to account for **~1/3 of the RAN market by 2029**
- Market expected to **exceed $10 billion** by end of the decade (Dell'Oro)

---

## Key distinction

Energy optimization AI is fundamentally different from generative AI. The article explicitly notes that "energy gains from optimization consistently outweigh computational overhead" — addressing the common objection that running AI in the network adds power consumption.

---

## Watch-outs

- Three UK's 70% figure is at "select sites" — likely low-traffic rural or nighttime sites
- 14% (Vodafone) is more representative of network-wide average in a mixed urban/suburban deployment
- All four operator examples use Ericsson solutions — Nokia and Huawei deployment data is less publicly available but exists
