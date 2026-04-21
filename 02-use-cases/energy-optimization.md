# Use Case: RAN Energy Optimization

*Last updated: April 2026*
*Maturity tier: Tier 1 — deploy now, ROI is real*

---

## What it is

AI-driven sleep mode management that dynamically powers down RAN components
(antenna branches, amplifiers, carriers) during low-traffic periods and wakes
them rapidly when demand returns. The AI monitors live traffic patterns and
acts autonomously — no human approval step once thresholds are configured.

**Deployment model:** AI on RAN — external system reads KPIs and instructs
the RAN to sleep. Not embedded in baseband. Works on any vendor's hardware.

---

## Why it's Tier 1

Three conditions that make this the highest-confidence RAN AI investment today:

1. **Multiple independent operator references** with named operators and real numbers
2. **Weeks to deploy** — no new hardware, no architectural change required
3. **Clear ROI** — energy costs are 7–10% of operator opex; RAN is 87% of that

---

## Confirmed operator results

| Operator | Country | Savings | Type |
|----------|---------|---------|------|
| Vodafone UK | UK | 14% per site (network-wide) | Production |
| Vodafone UK (2025 trial) | UK | Up to 33% at select London sites | Trial |
| Umniah | Jordan | 20% daily | Production |
| Far EasTone | Taiwan | 25% daily RAN energy | Production |
| Three UK | UK | Up to 70% at select sites | Trial |

**Read the table carefully:**
- 14% (Vodafone network-wide) is the conservative, replicable number — use this for business cases
- 33% and 70% are peak figures at optimal conditions, not network averages
- All named deployments use Ericsson solutions — Nokia and Huawei claim similar results but with fewer public references

---

## How it works (Vodafone example)

1. AI algorithms analyze baseband traffic patterns in real time
2. Compare projected vs. actual **PRB (Physical Resource Block) utilization**
3. If utilization is below threshold → autonomously power down antenna branches, amplifiers, or entire carriers
4. When traffic increases → wake up rapidly, restore full capacity

**Three sub-features in Ericsson's implementation:**
- **5G Deep Sleep** — full radio hibernation during very low traffic; up to 70% savings in those windows
- **4G Cell Sleep Mode Orchestration** — behavioral models per cell, balances sleep vs. coverage
- **Radio Power Efficiency Heatmap** — ML tool identifying underperforming sites for targeted tuning

---

## Deployment profile

| Dimension | Detail |
|-----------|--------|
| Deployment time | Weeks (software only, no hardware change) |
| Automation level | Closed-loop — AI acts autonomously |
| Hardware dependency | None — works on existing RAN |
| Data required | Live KPI streams, PRB utilization counters |
| Vendor lock-in | High — each vendor's solution uses proprietary data streams |

---

## AI maturity context

Most production deployments today are at **Stage 2** of the AI maturity journey:

| Stage | Description | Status |
|-------|-------------|--------|
| 1 | Rules-based (static sleep schedules) | Widely deployed |
| 2 | Autonomous — AI adapts dynamically to traffic | **Current production standard** |
| 3 | Cognitive intent-driven networking | In trials |
| 4 | Zero-touch lifecycle management | Roadmap |

Stage 1 (scheduling sleep at 2am) is not AI — it's automation. The distinction
matters when evaluating vendor claims.

---

## Business case anchor

- RAN = 87% of operator energy consumption (GSMA Intelligence)
- Energy = up to 10% of total opex
- At 14% RAN energy savings: roughly **1.2–1.4% opex reduction**
- Deployment cost: primarily integration and configuration (weeks of engineering time)
- Payback period: typically under 12 months at scale

---

## Watch-outs

- **Baseline problem:** vendors rarely disclose the traffic load or site density of reference deployments. A 20% saving at a lightly-loaded rural site does not replicate in a dense urban cluster at peak hours.
- **Coverage risk:** aggressive sleep modes can degrade coverage at cell edges. Confirm the vendor's coverage guard mechanism before deploying.
- **Vendor lock-in:** the AI reads proprietary counter streams. Switching vendors means re-integrating the AI layer.
- **"Up to" figures:** always ask for the average, not the peak. "Up to 70%" with a 14% network average means most sites are nowhere near 70%.

---

## Sources

- Ericsson / Vodafone UK press release (2025) — [operator case study]
- RCR Wireless: "How will AI drive 5G RAN energy efficiency?" (Oct 2024) — [independent]
- GSMA Intelligence: RAN energy consumption share — [standards body]
