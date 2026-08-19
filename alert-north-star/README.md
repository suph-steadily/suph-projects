# Alert North Star

**Owner:** Suph · **Started:** Aug 2026 · **Status:** Active, framing stage
**Last updated:** 2026-08-19 (say-back after Christina walkthrough + meeting record from LaNae 8/18, Julie 8/17, David 8/17, Brent 8/18)

---

## 1. The North Star

- The North Star is not "zero alerts" for its own sake. It is **growth without agent friction**: grow the business safely, with no underwriting roadblocks in the agent's path.
- Zero underwriting alerts is the measurable expression of that. Alerts protect the book, but they lose binds: ~70-77% of quotes drop off when the dwelling age alert fires, and only ~15% of referred quotes bind.
- Leadership already agrees on direction, in slightly different words:
  - **Christine:** zero underwriting alerts; the real fix may be better upstream pre-fill data, not per-alert patches.
  - **LaNae:** zero underwriting alerts for older dwellings. Strong preference to say no or add friction upfront rather than cancel post-bind.
  - **Darren:** top-line growth is the priority, not UW efficiency ("can hire more underwriters if referral volume increases").
  - **Brent's benchmark:** Travelers runs ~93% available-for-issue in small commercial; Steadily is ~60%.
- **First exploration: killing our biggest alert, dwelling age (101+).** Everything below is built around that worked example.

## 2. The synthesis (where this actually stands)

- The two threads are one loop, not two projects. **The weighing machine is the scale; automating the underwriter's jobs moves the weights.** Weigh the alert → if too expensive to remove, automate the UW job driving the cost → re-weigh → kill it.
- Alignment is not the blocker. Nobody has named a **price**. There is no agreed bind-to-knock ratio, and LaNae has never been asked what knock increase she would accept. That number is the gate for everything else.
- Per David: **the bind lift is already proven.** The entire open question is the cost side (knocks, NOEs, lost premium, agent attrition). So the weighing machine is really a cost-pricing machine.
- One correction to how I said it out loud: "removing the alert has no NOC/NOE impact, only upside" is **not what the data says**. Raw removal roughly doubles the knock rate (~3.9% → ~7.5-8%). The defensible claim is: **with the three levers implemented, incremental knocks approach zero** because we automate what the UW was doing. Thursday's proposal has to say the second version, not the first.
- The rubber-stamp rate (~45% today, ~80% if the levers land) is a **gauge** of residual UW value, not the finish line. At 80%, the machine's question becomes: does the remaining 20% of interventions beat the friction cost imposed on 100% of quotes?
- The machine works both directions: alerts to **remove** and proposed alerts to **add**.

## 3. The Weighing Machine

What it must output for any alert:

- How many more binds
- How many more NOCs / NOEs (post-bind letters)
- GWP gained or lost
- Loss-ratio impact (lagging; counts are the leading proxies)

Status after the LaNae 8/18 meeting, this is no longer a mystery project:

- **Partner:** David Curry (or the new data scientist) builds the bind/knock trade-off model with me.
- **Named inputs:** bind LTV, knock cost / agent impact, and UW premium contribution per alert.
- **Output shape:** a shared threshold any alert can be evaluated against (e.g., "100:1 bind-to-knock" as an illustrative bar).
- **Form:** start as a repeatable analysis template, not a UI project. `tradeoff.py` in scratch-darren is the first module. Decide doc vs. script vs. tool after running it manually 2-3 times.
- Context for urgency: the overall knock rate is already creeping toward 10%, so appetite for more knocks is limited.
- Still unpriced: the four weights from the What Must Be True framing, including agent-loss from knock experience (partially sized now, see §6).

## 4. Worked example: dwelling age (weighing machine v0)

What underwriters do with the 101+ alert today:

- **~45%: nothing.** Approve with no changes. (Rubber stamp.)
- **~15%: add a roof exclusion.**
- **~10%: roof exclusion plus something else.**
- **~30%: data corrections.** Six property attributes (square footage, unit count, etc.) account for ~90% of the edits.
- **Soft checks:** historic district, commercial use, sober-living. Accept or reject on those grounds.

What removal costs and gains (91-100 cohort layered onto 101+):

- **~115 more binds/month, ~25 more knocks/month, ~30 more NOEs/month.** Knock rate roughly doubles (~3.9% → ~7.5-8%).
- **~$26K/month (~$310K/yr, ~$67/policy)** in premium-bearing corrections UWs make at review time would go un-captured. Note: a separate $1.4M annual figure is floating around; these do NOT reconcile yet ($26K x 12 ≈ $310K). Resolve before Thursday.
- Evidence the alert adds little risk-screening value: the pre-alert cohort (pre-March 2025) and the 95-year-old cohort both behave identically to the general population.

## 5. The levers (the UW's jobs, plus one alternative)

| Lever | What UWs do | Gap today | What it requires |
|---|---|---|---|
| **Roof exclusion aperture** | Apply roof exclusion to ~20% of 101+ dwellings | Auto-RSE (imagery + roof score, age is NOT an input) fires on only **3.5%** of them; model targets bottom 5% overall | Expand threshold with Curry: bottom 5% → **bottom 20%**, longer-term bottom 40% for older homes. Live nationwide ~2 weeks, 7 states ~2 months. **Texas has the longest bake, check it first.** |
| **Pre-fill data quality** | Correct our own pre-filled attributes by hand (LandGlide, Zillow, county records) | Agents game pre-fills (about -$200 premium); UWs are ~4x more likely to re-correct agent-touched values (adding it back plus ~$30). Roof type: 48% of final values differ from both pre-fill and agent input | Source data the way UWs do. Build the case for LandGlide or equivalent API, starting with 101+ homes. Bake-off underway (Smarty today, price SmartSource). |
| **Commercial / historic / sober-living** | Soft-check and accept/reject | Manual today | Pluribus/Spotlight LLM historic-district signal expected **~October** (on hold until then). Alternative: agent attestation questions in the flow, Brent says agents prefer upfront questions over UW referral. |
| **Premium age modifier (alternative)** | n/a | Instead of fixing every attribute, price the uncertainty: ~10% bump for 100+ homes, reduced as accuracy improves | Actuarial input ("what rate adjustment maintains loss ratio if corrections never happen?"). Admitted states need rate filings, so not a fast follow. |

- If the first three land, expect rubber-stamp to go from ~45% to ~80%.
- Correction from my dictated version: the auto exclusion rate on 101+ is **3.5%**, not "bottom 5%." The 5% is the model's overall aperture; on this cohort it fires 3.5% vs. UWs' ~20%.

## 6. Why knocks are the sensitive weight (Darren + LaNae)

- **Darren's early data: 3 knocks = you lose the agent permanently** (not just the policy). First knock that proceeds to cancellation correlates with **~20% drop in future buys from that agent**. Early data points, not published figures.
- **LaNae's two reasons knocks sting:** the bait-and-switch perception (the deal changed after bind) and the remediation burden (legally required to specify exactly what must be fixed).
- UAR was tried as a pre-cancellation softener and abandoned: agents read it as a cancellation anyway.
- We know the three-strike rule of thumb but not the full underlying mechanism. The scratch-darren curves (74% of NOCs cure; cured knocks cost ~0 to -2% per event vs. cancellations at -20% front-loaded) are the start of the evidence.
- The optimistic logic: if we do the UW's jobs really well, **the knock rate should not change** relative to today. The same interventions happen, just automated pre-bind. Residual knocks exist today with a UW in every single 101+ journey, so they are a standing optimization target **whether or not the alert lives.**

## 7. Missing dots and risks (the honest list)

- **Selection effect.** Cohort layering assumes the same population applies after removal. Alerts also deter bad risks from submitting. The knock projection is a **floor**, not an estimate.
- **The 45% do-nothing is not free.** Part of its value is deterrence, and the catch rate on intervened policies is real (6.8% vs. 3.3% baseline). Price it, don't dismiss it.
- **Loss ratio is lagging.** Counts (binds, knocks, NOEs, cancellations) are the leading proxies, which is why the machine is denominated in them.
- **Terminology is unresolved.** NOC vs. NOE vs. UAR get used loosely (an NOE rate can exceed the NOC rate, they are separate letters). Sync definitions with LaNae before reporting numbers. All three are letters and post-bind.
- **$1.4M vs. $310K** premium-recapture figures do not reconcile yet. Fix before Thursday.
- **Reference points to reuse:** knock/NOE baseline ramps ~+22.8% per decade of dwelling age (30-60yr baseline: NOC 2.75 / NOE 5.11 per 100). Foregone premium from un-run UW edits ~$3,034 per 100 bound, ~0% recovered at renewal.

## 8. The proposed test (Julie's design + David's sequencing)

- **Design:** 50/50 A/B in the top 7 states (bind-to-knock ratio above 8.8:1 for 90-100yr homes; Illinois example: 21% bind rate). ~30-60 days for significance. Not a full toggle.
- **Framing (Julie):** don't ask underwriting yes/no. Ask them to help **size the risk**: here's the upside, here's the bounded downside. The business decision then goes to Darren or Christine. Include roof-exclusion progress as a mitigant.
- **Sequencing (David):** the bind lift is proven, so ask LaNae what knock/NOE increase she'd accept **before** running the test. If the answer is effectively "none," skip the test and work the levers first.

## 9. Open questions

- **The gate:** what knock increase will LaNae accept? Never asked directly.
- What is one bind worth vs. one knock, in dollars? (Bind LTV, knock cost, agent attrition = the Curry model inputs.)
- **Unexplored thread: what are 101+ policies actually getting knocked FOR today?** We have rates but never pulled reasons. If residual knock reasons overlap the three levers, automation shrinks them too. If not, that is new information about what the alert does and doesn't protect against.
- What share of NOEs for 90-100yr homes are roof exclusions? If ~50%+, strong case to retarget the model for this cohort now.
- Does Texas (longest roof-model bake) already show the manual 15% roof-exclusion rate dropping?
- Once the levers land and rubber-stamp hits ~80%, does the machine say kill? Run it, don't assume.
- Which alert goes second?

## 10. Next steps

- [ ] **Thursday 8/20: proposal for the underwriting sync.** Dwelling age as weighing machine v1: bind upside, bounded downside, three levers as mitigants. Framed as risk-sizing, not a yes/no ask. Decision routes to Darren/Christine.
- [ ] **Ask LaNae her tolerance number** (or make it the explicit ask inside the Thursday proposal).
- [ ] Reconcile $1.4M vs. $310K before Thursday.
- [ ] Meet David Curry: scope the bind/knock model (bind LTV, knock cost, UW premium per alert).
- [ ] With Curry: expand roof-exclusion threshold to bottom 20% for older dwellings.
- [ ] Check Texas knock/NOE data for roof-model impact.
- [ ] Pull knock reason codes for the 101+ cohort.
- [ ] Build the LandGlide (or equivalent) pre-fill sourcing case, starting with 101+ homes; get SmartSource pricing.
- [ ] Explore the premium age modifier with actuarial.
- [ ] Review the conditional liability exclusion form (eng + comms lift).
- [ ] Validate Darren's three-knock rule when his final figures land.

---

## Appendix: sources

Published artifacts (mine):

- [What Must Be True](https://claude.ai/code/artifact/17b10280-3546-453c-8e9a-b6ee95c16ae8) — the weighing machine framing, v4 (8/18). Source HTML copied to [appendix/what-must-be-true.html](appendix/what-must-be-true.html).
- [Two Houses](https://claude.ai/code/artifact/443f560b-9bf3-4e43-be04-27f79d015393) — dwelling age baseline; the +22.8%/decade ramp is in §2.
- [The Dwelling Age Plan](https://claude.ai/code/artifact/b2948c69-3b9e-4eaf-a0d5-01ea005152fe) — the 8/17 project plan.
- [Why Old Roofs Slip Through](https://claude.ai/code/artifact/cc9e164c-8f41-40d1-8d02-99e3b152a43e) — the roof exclusion aperture gap (3.5% vs 20%).
- [DAMR — pre-fill flip validation](https://claude.ai/code/artifact/6b9d96a2-67b1-496f-9f08-ce2d9fbda1b6) — agent-touched values corrected 2-6x more (8/7).
- [The Pre-fill Bake-off](https://claude.ai/code/artifact/f743c2ea-d0a3-4555-8c6b-41eddcaf69b5) — provider comparison design (8/17).

Shared with me:

- [Dwelling-Age Pre-Bind Review — Findings (Aug 2026)](https://claude.ai/code/artifact/cd621252-2b1d-4e3e-a088-4b55592d24be)

Other sources:

- [scratch-darren NOC impact report](https://github.com/landlordhq/scratch-darren/blob/main/2026-08-noc-impact-inde-agent/REPORT.md) — cure rates, retention curves, `tradeoff.py`.
- [appendix/damr_prefill_flip_briefing_v8.pdf](appendix/damr_prefill_flip_briefing_v8.pdf)
- [appendix/damr-end-to-end-briefing.pdf](appendix/damr-end-to-end-briefing.pdf)

Meetings (Granola):

- LaNae / Suph, Aug 18 — knock pain, Darren's data, Curry weighing-machine plan
- Suph / Brent, Aug 18 — AFI benchmark, attestation-questions idea
- David / Suph, DAMR Alert Test, Aug 17 — bind lift proven, tolerance-first sequencing
- Julie / Suph, Aug 17 — 50/50 test design, risk-sizing framing, age modifier
- Suph // Christine 1:1, Aug 13 — zero-alerts north star, upstream pre-fill thesis
- Product / UW Sync, Aug 6 — Pluribus/Spotlight October timeline

Local working folders (this laptop): `~/mockups/alert-north-star/`, `~/mockups/dwelling-age-project-plan/`, `~/mockups/damr-*/`.
