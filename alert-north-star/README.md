# Alert North Star

**Owner:** Suph · **Started:** Aug 2026 · **Status:** Active, framing stage
**Last updated:** 2026-08-19 (say-back after Christina walkthrough)

---

## 1. The North Star

- The North Star is not "zero alerts" for its own sake. It is **growth without agent friction**: grow the business safely, with no underwriting roadblocks in the agent's path.
- Zero underwriting alerts is the measurable expression of that. Alerts put a roadblock in front of the agent; that protects the book, but it loses binds: people give up in the review process or get scared off.
- The question this project answers: **what needs to be true for us to have zero alerts?** (Or at least: for any single alert, should it exist?)
- **First exploration: killing our biggest alert, dwelling age (101+).** Everything below is built around that worked example.

## 2. The two threads, and how they connect

There are two threads that kept feeling separate. They are not. One is the scale, the other moves the weights.

- **Thread 1: The Weighing Machine.** A repeatable way to price an alert: binds gained vs. NOCs/NOEs/lost premium if we remove it. It does not kill alerts by itself. It tells you the price.
- **Thread 2: Automating the underwriter's jobs.** Roof exclusions, pre-fill corrections, commercial/historic checks. This does not kill alerts either. It **lowers the cost side** of removal, so the machine's verdict can flip from "too expensive" to "remove it."

The loop:

> **Weigh the alert → if too expensive to remove, automate the underwriter job driving the cost → re-weigh → kill it.**

- The rubber-stamp rate (how often UWs approve with zero changes) is the **gauge** of residual UW value, not the finish line.
- At an 80% rubber-stamp rate the question is not "can we kill it?" It's: does the value of the remaining 20% of interventions beat the friction cost imposed on 100% of quotes? That is a number the machine produces, not a judgment call.
- The machine works both directions: evaluating alerts to **remove** and proposed alerts to **add**.

## 3. The Weighing Machine

What it needs to output for any alert (the holy trinity plus one):

- How many more binds
- How many more NOCs / NOEs (post-bind letters)
- GWP gained or lost
- Loss-ratio impact (lagging; counts are the leading proxies)

Denomination: counts on one side, dollar weights on the other. This was largely settled in the "What Must Be True" v4 framing (see appendix): 6 counts vs. 6 weights, everything reducing to expected dollars but presented with counts because that is what the team trusts.

Open questions on the machine itself:

- **Form:** UI tool? Model? Recurring analysis? Start as a **repeatable analysis template**, not a UI project. `tradeoff.py` in scratch-darren is the first module.
- **The four unpriced weights** (incl. agent-loss from NOC experience) still need dollar values.
- Whether the machine is truly a prerequisite to continuing, or whether we can keep acting alert-by-alert while it matures. (Current view: the dwelling age analysis IS the machine, v0, run by hand once. "Build the machine" = codify that analysis, not start a system project.)

## 4. Worked example: dwelling age (weighing machine v0)

The 101+ dwelling age alert forces a manual UW review. What underwriters actually do with it:

- **~45%: nothing.** Approve with no back-and-forth, no changes.
- **~15%: add a roof exclusion.**
- **~10%: roof exclusion plus something else** (usually property attribute edits).
- **Six property attributes account for ~90% of the edits** (square footage, unit count, etc.).
- **Soft checks:** historic district, commercial use, sober-living. Accept or reject on those grounds.

What removing the alert costs/gains (cohort layering: 91–100 year old behavior applied to 101+):

- **~115 more binds, ~25 more cancellations, ~30 more endorsements.**
- Generally no impact to NOC/NOE rates in the projection, because at best we replicate what UWs do.
- NOCs/NOEs already happen today even WITH a UW in every one of these journeys, so zero incremental is the target, not zero absolute.

## 5. The three automation levers (the UW's jobs)

| Lever | What UWs do | Gap today | What it requires |
|---|---|---|---|
| **Roof exclusion aperture** | Apply roof exclusion to ~20% of 101+ dwellings | Auto-RSE (imagery + roof score) fires on only **3.5%** of them | Model/threshold change. Note: auto-RSE triggers on roof score ONLY, age is not an input today. |
| **Pre-fill data quality** | Correct our own pre-filled attributes (the six fields) | UWs correct agent-touched values **2–6x more** (by field) than untouched pre-fills; agents game the pre-fills to their benefit | Better source data. Provider bake-off underway (Smarty today; price SmartSource first). |
| **Commercial / historic / sober-living** | Soft-check and accept/reject | Manual today | Pluribus team automation, and/or agent attestation questions in the flow ("is this a historic district? commercial? sober-living?") |

- If all three land, expect the rubber-stamp rate to go from ~45% to something like ~80%.
- Correction from the dictated version: the auto exclusion rate on 101+ is **3.5%**, not "bottom 5%." The 5% figure is the model's overall aperture; on this cohort it fires 3.5% of the time vs. UWs' ~20%. That gap is the opportunity.

## 6. Missing dots and risks (the honest list)

- **Selection effect.** The 91–100 cohort layering assumes the same population applies after removal. Alerts also deter bad risks from submitting at all. Removal changes who shows up, so the NOC projection is a **floor**, not an estimate.
- **The 45% do-nothing is not free.** Part of its value is deterrence, and the catch rate on intervened policies is real (6.8% vs. 3.3% baseline). Price it, don't dismiss it.
- **Loss ratio is lagging.** It takes years to read. Counts (binds, NOCs, NOEs, cancellations) are the leading proxies, which is why the machine is denominated in them.
- **Known reference points to reuse:** NOC/NOE baseline is not flat, it ramps ~+22.8% per decade of dwelling age (30–60yr baseline: NOC 2.75 / NOE 5.11 per 100). 74% of NOCs cure; cured NOCs cost roughly 0 to -2% per event vs. cancellations at -20% front-loaded. Foregone premium from un-run UW edits is ~$3,034 per 100 bound, ~0% recovered at renewal.

## 6a. Why NOCs are the sensitive weight (Darren)

- The folk rule: **knock an agent three times and you lose them forever.** That is why every team is so NOC-sensitive.
- We know the rule of thumb, but we do NOT know the underlying mechanism. This is exactly the "agent-loss NOC" unpriced weight. The scratch-darren retention curves (cured vs. uncured, front-loaded cancellation damage) are the start of the underlying evidence, but the three-strike threshold itself is unvalidated.
- The optimistic logic: if we do the UW's three/four jobs really well, **the NOC rate fundamentally should not change.** The same interventions happen, just automated pre-bind instead of by a human. If the UW team ends up rubber-stamping dwelling age referrals, the residual NOCs were happening anyway.
- Which means: the residual NOCs are a standing optimization target **independent of whether the alert lives or dies.** We have NOCs today with a UW in every single 101+ journey.

## 7. Open questions

- Once the three levers land and rubber-stamp hits ~80%, does the machine say kill? (Likely yes for dwelling age, but run it, don't assume.)
- What is 115 more binds actually worth in dollars vs. 25 cancellations + 30 endorsements? (This is exactly the four-unpriced-weights work.)
- Does the machine live as a doc template, a script (`tradeoff.py`-style modules), or eventually a tool? Decide after it has been run manually 2–3 times on different alerts.
- Which alert goes second after dwelling age?
- **Unexplored thread: what are 101+ dwelling age policies actually getting NOC'd FOR today?** We have the rates but never pulled the reasons. If the residual NOC reasons overlap with the three levers (roof, attributes, commercial/historic), automation shrinks them too. If they don't overlap, that is new information about what the alert is and isn't protecting against.

## 8. Next steps

- [ ] **Thursday 8/20: Julie proposal.** Write the dwelling age alert as weighing machine v1: one alert, fully priced, with the three automation levers shown as cost-reducers on the same page. This advances both threads at once.
- [ ] Reconcile the two sizing figures floating around ($1.4M vs. $310k, and 4.1% vs. 2%) before Thursday.
- [ ] Price the four unpriced weights (or explicitly mark them "unpriced, direction known" in v1).
- [ ] Pre-fill bake-off: get SmartSource pricing.
- [ ] Check with Pluribus team on commercial/historic/sober-living automation timeline.
- [ ] Pull NOC reason codes for the 101+ cohort (the unexplored thread above).
- [ ] Validate (or size) Darren's three-knock rule: is agent attrition after 3 NOCs real, and what is one NOC actually worth in future submissions?

---

## Appendix: source analyses

Published artifacts (mine):

- [What Must Be True](https://claude.ai/code/artifact/17b10280-3546-453c-8e9a-b6ee95c16ae8) — the weighing machine framing, v4 (8/18). Source HTML copied to [appendix/what-must-be-true.html](appendix/what-must-be-true.html).
- [Two Houses](https://claude.ai/code/artifact/443f560b-9bf3-4e43-be04-27f79d015393) — dwelling age baseline; the +22.8%/decade ramp is in §2.
- [The Dwelling Age Plan](https://claude.ai/code/artifact/b2948c69-3b9e-4eaf-a0d5-01ea005152fe) — the 8/17 project plan.
- [Why Old Roofs Slip Through](https://claude.ai/code/artifact/cc9e164c-8f41-40d1-8d02-99e3b152a43e) — the roof exclusion aperture gap (3.5% vs 20%).
- [DAMR — pre-fill flip validation](https://claude.ai/code/artifact/6b9d96a2-67b1-496f-9f08-ce2d9fbda1b6) — agent-touched values corrected 2–6x more (8/7).
- [The Pre-fill Bake-off](https://claude.ai/code/artifact/f743c2ea-d0a3-4555-8c6b-41eddcaf69b5) — provider comparison design (8/17).

Shared with me:

- [Dwelling-Age Pre-Bind Review — Findings (Aug 2026)](https://claude.ai/code/artifact/cd621252-2b1d-4e3e-a088-4b55592d24be)

Other sources:

- [scratch-darren NOC impact report](https://github.com/landlordhq/scratch-darren/blob/main/2026-08-noc-impact-inde-agent/REPORT.md) — cure rates, NOC/cancellation retention curves, `tradeoff.py`.
- [appendix/damr_prefill_flip_briefing_v8.pdf](appendix/damr_prefill_flip_briefing_v8.pdf)
- [appendix/damr-end-to-end-briefing.pdf](appendix/damr-end-to-end-briefing.pdf)

Local working folders (this laptop): `~/mockups/alert-north-star/`, `~/mockups/dwelling-age-project-plan/`, `~/mockups/damr-*/`.
