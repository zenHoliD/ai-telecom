# Operator Experience — Personal Knowledge Capture

*This file is yours to fill in. It is not derived from any external source.*
*The guide's differentiated value comes from this section more than any published source.*
*Fill it out before writing each use-case file. Your experience is the primary source.*

---

## How to use this file

For each use case, answer from direct experience: things you have personally seen deployed, configured, operated, or troubleshoot in a live network. Be specific — network type, vendor, geography, timeframe.

If you haven't seen something deployed: say so. "Not deployed in my network" is valuable signal.
If you've heard about it from a peer operator: note it as hearsay, not firsthand.

**Format:** Bullet points, rough notes. This is a working document, not a polished reference.

---

## Energy Optimization

**Have you seen this deployed in production in your network?**
<!-- Yes / No / Partial — describe scope -->

**Vendor and product used:**
<!-- e.g., Ericsson Energy Saver, Nokia AVA Energy Efficiency, vendor-agnostic third party -->

**What results did you actually observe?**
<!-- Actual savings figure, how measured, what baseline was used -->

**What didn't work as advertised?**
<!-- Coverage gaps, reactivation latency, interference after wake-up, NOC pushback -->

**What would you tell a colleague at another operator considering this?**
<!-- The one thing the vendor deck doesn't say -->

---

## Beam Management

**Have you seen AI-assisted beam management deployed in production?**
<!-- Yes / No / Partial — describe scope, if massive MIMO or specific bands -->

**Vendor and product used:**

**Observed throughput improvement:**
<!-- Actual figure, what the baseline was, UL vs. DL, dense urban vs. suburban -->

**Hardware dependency:**
<!-- Required massive MIMO? Specific bands? Specific generation of baseband? -->

**What the vendor claimed vs. what you observed:**

**One thing you'd warn a colleague about:**

---

## Predictive Fault Detection

**Have you seen AI-based fault prediction deployed in your NOC?**
<!-- Yes / No / Partial — describe what domain (RAN, transport, core) -->

**What tool:**
<!-- Vendor product or in-house? Integrated with ticketing system? -->

**False positive rate in first 90 days:**
<!-- If you remember — this is the critical operational metric -->

**Did the NOC actually use it, or was it bypassed?**
<!-- Honest answer. If bypassed, what caused the trust breakdown? -->

**MTTR change observed:**
<!-- Actual figure or qualitative — better, same, worse -->

**What changed organizationally to make it work (or what failed organizationally)?**

---

## Capacity Planning / Traffic Prediction

**Does your OSS vendor provide AI-assisted capacity planning?**
<!-- Yes / No — and do you actively use it? -->

**If yes: does it actually influence the quarterly capex review?**
<!-- Or is it a dashboard no one looks at -->

**Where the model works well:**
<!-- Dense urban? Rural? Specific traffic profiles? -->

**Where the model breaks:**
<!-- Events, new developments, seasonal spikes it didn't predict -->

---

## O-RAN / xApps

**Does your network have any O-RAN deployed? If yes, which category:**
<!-- A (open hardware), B (RIC trial), C (RIC primary) — use Analysys Mason's categories -->

**Which vendor(s) for RU / DU / CU / RIC:**

**If a RIC is deployed: which xApps are running?**

**Integration timeline you experienced vs. what was estimated:**

**The hardest part of the O-RAN deployment that no one talks about:**

---

## Failure Modes — Personal Experience

For each failure mode, note: have you seen it? At what stage? What was the outcome?

| FM | Description | Seen it? | Stage it hit | What happened |
|----|-------------|----------|--------------|---------------|
| FM-1 | Data quality / counter inconsistency | | | |
| FM-2 | Trial-to-production gap | | | |
| FM-3 | Vendor proprietary data lock-in | | | |
| FM-4 | xApp conflict | | | |
| FM-5 | NOC trust / adoption failure | | | |
| FM-6 | Baseline measurement impossibility | | | |
| FM-7 | Model drift | | | |
| FM-8 | Integration complexity underestimated | | | |

**The failure mode that's most underrepresented in the published literature:**
<!-- The thing you've seen that doesn't appear in any analyst report -->

---

## Vendor Assessment (from operational experience)

*Only answer what you have direct experience with. Skip vendors you haven't worked with.*

**Ericsson:**
<!-- What their AI features actually do in production vs. what they demo. What's overstated. -->

**Nokia:**

**Huawei:**

**Other vendors (Mavenir, Samsung, etc.):**

---

## The One Insight That Isn't in Any Source

*What's the thing you know about RAN AI in production that you've never seen written down?*
*This is the most valuable content in the entire guide.*

<!--
Write it here. Even if it's a hunch. Even if it contradicts a published source.
This is the section that makes the guide worth reading for someone who already
knows the vendor decks.
-->
