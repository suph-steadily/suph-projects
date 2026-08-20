# Zero Alerts

**Owner:** Suph · **Started:** Aug 2026 · **Status:** Active, framing stage
**Last updated:** 2026-08-19

**This project has three pieces:**

| Piece | What it is | Where |
|---|---|---|
| **This doc** | The strategy: thesis, the UW's four jobs, the dwelling age worked example, terminology, open questions | you're reading it |
| **The Weighing Machine** | Sub-project: the model that prices any alert (binds gained vs NOCs/NOEs/premium lost). Requirements + first-pass Python, 26 passing tests | [weighing-machine/](weighing-machine/REQUIREMENTS.md) |
| **The deck: Killing the Dwelling Age Alert** | The presentable version, 13 slides, arrow keys to advance | [deck](https://claude.ai/code/artifact/ee09fae6-e609-423e-ba7f-fd3d58332bcf) · [source](deck.html) |
| **The page** | The same story as a scrolling walkthrough | [page](https://claude.ai/code/artifact/f9cc6eb3-5a61-41d8-9d7c-3e485e2404d1) · [source](thesis.html) |

---

## 1. The North Star

- The North Star is not "zero alerts" for its own sake. It is **growth without agent friction**: grow the business safely, with no underwriting roadblocks in the agent's path.
- Zero underwriting alerts is the measurable expression of that. Alerts protect the book, but they lose binds. **Out of 100 quotes whose only obstacle is the dwelling age alert: 63 never get referred (the roadblock scares the agent off before any human looks), 21 get referred but never bind, and 16 become policies.** The loss happens before the underwriter ever sees the quote, not in the decision: referrals that do get approved go on to issue at 46-51%.
- Leadership already agrees on direction, in slightly different words:
  - **Christine:** zero underwriting alerts; the real fix may be better pre-fill data at the source, not per-alert patches.
  - **LaNae:** zero underwriting alerts for older dwellings. Strong preference to say no or add friction upfront rather than cancel after bind.
  - **Darren:** top-line growth is the priority, not UW efficiency ("can hire more underwriters if referral volume increases").
  - **Brent's benchmark:** Travelers runs ~93% available-for-issue in small commercial; Steadily is ~60%.
- **First exploration: killing our biggest alert, dwelling age (101+).** Everything below is built around that worked example.

## 2. The synthesis (where this actually stands)

- The two threads are one loop, not two projects. **The weighing machine is the scale; automating the underwriter's jobs moves the weights.** Weigh the alert → if too expensive to remove, automate the UW job driving the cost → re-weigh → kill it.
- Alignment is not the blocker. Nobody has named a **price**. There is no agreed bind-to-NOC ratio, and LaNae has never been asked what NOC increase she would accept. That number is the gate for everything else.
- Per David: **the bind lift is already proven.** The entire open question is the cost side (NOCs, NOEs, lost premium, agents walking away). So the weighing machine is really a cost-pricing machine.
- **The thesis, stated properly:** automate the four jobs the underwriter does on this alert (approve, roof exclusion, data corrections, eligibility soft-checks) so the system duplicates the UW exactly. THEN turn off the alert. Expected result: **no increase in NOCs or NOEs**, because the same interventions still happen, just before bind and automated. If that holds, the alert can go.
- How close to zero? Today the unreviewed twin gets about **2 extra NOCs per 100 policies** vs the reviewed book (6.5 vs 4.5). Of those 2: about **1.5 come from things our levers automate** (roof, eligibility, data). The last ~0.5 comes from things the on-site inspection finds, which the UW review doesn't prevent today either. So with working levers, removal adds close to zero — not exactly zero. (Full reason-by-reason breakdown in §6b.)
- Put positively: **the underwriter HAS been protecting the book** — about 2 NOCs per 100 prevented — and that protection sits almost entirely in the jobs we are automating (roof, eligibility, honest data). The chart is on the deck's "protecting the book" slide.
- **Automation alone buys zero growth — it only defends the NOC rate.** The 63 of 100 who die at the roadblock never even submit; automating what happens behind the alert recovers none of them. They come back only when the alert itself disappears. So: automation is the hedge, removal is the unlock. Both, in that order.
- **And the carry-over has to be proven, not assumed.** Once the levers exist, remove the alert for a small slice (say 10%) and watch whether the systems truly duplicate the UW: NOC and fix rates on the un-gated slice should hold at reviewed-book levels. That is the test that makes sense: a 10% test AFTER the levers are built, not a 7-state discovery test before them.
- **The leftover NOCs carry over, and they're the next frontier.** Even with a UW reviewing every 101+ bind, the reviewed book still runs ~4.5 NOCs and ~3.9 NOEs per 100 bound (within 90 days). Duplicating the UW keeps that rate — it doesn't fix it. Not great. §6b says what those leftover NOCs are made of (mostly Condition-General and Liability findings from the post-bind inspection), so improving it is a different problem than the alert: condition information before bind (imagery or an inspection), not data review. That optimization is worth doing whether or not the alert lives.
- **The goal: "approve, touching nothing" goes from 46.5% to 90% or better.** The underwriter opens the referral and finds nothing left to do, because the system already did it. That climb is the gauge that the fixes work, measured while the alert is still on and nothing can go wrong. It is a gauge, not the finish line. At 80%, the machine's question becomes: does the remaining 20% of interventions beat the friction cost imposed on 100% of quotes?
- The machine works both directions: alerts to **remove** and proposed alerts to **add**.

## 3. The Weighing Machine

**Sub-project: [weighing-machine/](weighing-machine/REQUIREMENTS.md)** — requirements, inputs, validation plan, and the first-pass Python model live there.

What it must output for any alert:

- How many more binds
- How many more NOCs / NOEs (as defined in the Terminology section: UW cancellation events and UW corrective endorsement transactions, not letter counts)
- GWP gained or lost (gross written premium: the total premium dollars)
- Loss-ratio impact (slow to show up; the counts above move first, so we watch them instead)

Status after the LaNae 8/18 meeting, this is no longer a mystery project:

- **Partner:** David Curry (or the new data scientist) builds the bind/NOC trade-off model with me.
- **Named inputs:** bind LTV (lifetime value: what one bound policy is worth over its whole life), NOC cost / agent impact, and UW premium contribution per alert.
- **Output shape:** a shared threshold any alert can be evaluated against (e.g., "100:1 bind-to-NOC" as an illustrative bar).
- **Form:** start as a repeatable analysis template, not a UI project. `tradeoff.py` in scratch-darren is the first building block. Decide doc vs. script vs. tool after running it manually 2-3 times.
- Context for urgency: the overall NOC rate is already creeping toward 10%, so appetite for more NOCs is limited.
- Still unpriced: the four weights from the What Must Be True framing, including agent-loss from NOC experience (partially sized now, see §6).

## 4. Worked example: dwelling age (weighing machine v0)

What underwriters do with the 101+ alert today (10,428 approvals we could analyze):

- **46.5%: nothing.** Approve with no change of any kind. (Rubber stamp.)
- **14.0%: roof-surfacing exclusion only.**
- **11.3%: roof exclusion plus something else** (1.1% roof detail corrections, 5.5% property corrections, 4.7% coverage changes).
- **28.2%: changes without a roof exclusion** (mostly data corrections). Six property fields carry ~90% of the corrections (~95% of pure attribute edits).
- **8.4% of asked referrals are rejected** — the gate's real defense; 82% of rejections are commercial / historic / unit-count, not age itself. "Real judgment" is ~11% of approvals.

What removal costs and gains (the 91-100 sister cohort's rates applied to the 101+ homes):

- **~+115 binds/month** (range +50 to +175; top-7-states cut = +107), **~+25 NOCs/month** (range +10 to +40), **~+30 UW corrective endorsements/month**, and ~2,200 forced UW reviews/month eliminated.
- **Where the +115 really comes from (re-confirmed 8/19, card 141815):** the age-alert lane binds at 15.7%. Matching state by state, just-under no-alert homes in the same states bind at rates that would put the alerted homes at **17.7%** — that gap is the ~115 binds a month (reproduces today as +112 across 30 states). One trap the analysis already solved: never use the national just-under average (13.7 to 14.4%) — the volumes sit in different states, and pooled nationally it points the wrong way; within states, no-alert old homes bind HIGHER than the alerted lane. For scale, clean under-91 homes bind at 18.0%. The card's own caveat stands: the counterfactual assumes the gaps are gate-caused, so it is an upper-bound read that the 10% test would confirm.
- **The rates:** NOC 4.5% → 6.5% (up 1.9 points); UW-fix rate 3.9% → 7.5% (up 3.6 points, ~2x). Caveat: the twin's 6.5 is a floor: the 101+ book averages 117 years, and projecting the age trend forward gives an un-reviewed NOC rate of 7-11 per 100 (7-11%).
- **~$26K/month premium from UW corrections** would go un-captured (bound-only; fixes raise price 3.4x more often than they lower it; $3,034 per 100 bound, never recovered at renewal). **Use $310K/yr** (bound-only, real collected revenue; ~$30 per bound policy). The $1.4M figure counts corrections on all approved quotes, 78% of which never bound. Strict floor: ~$270K/yr after subtracting the ~12% the sister book recovers after bind.
- Evidence the alert adds little risk-screening value: the homes from before the alert existed (pre-March 2025) and the 95-year-old homes both behave identically to the general population.

## 5. The levers (the UW's jobs, plus one alternative)

| Lever | What UWs do | Gap today | What it requires |
|---|---|---|---|
| **Roof exclusion dial** | Hand-apply the exclusion to **21.3 per 100** of 101+ referrals (vs. the robot's 3.5, a 6:1 split; 1,144 referrals, 7 states May-Jun) | Auto-RSE (imagery + roof score; age is NOT an input) actions only ~4% of old-home referrals (3.8% exclude + 0.1% alert). UWs start excluding around score 69, typical case 83 | Lower the score bar for old homes; **underwriting sets the dial.** Priced dial: score 90+ catches 33% of UW hand-applies at 1.3 over-applies per catch · 80+ = 57% at 1.8 · 70+ = 74% at 2.1 · 60+ = 84% at 2.4. Live nationwide ~2 weeks, 7 states ~2 months. **Texas has the longest bake, check it first.** |
| **Pre-fill data quality** | Correct our own pre-filled attributes by hand (LandGlide, Zillow, county records) | UWs correct agent-touched fields **2-6x more** than untouched pre-fill (sq ft 25.3% vs 4.5% = 5.7x; year built 5.6x; roof/construction 1.4-1.6x; property type flat) — yet **65-79% of UW fix volume lands on pre-fill the agent never touched**. Gaming signature: 210 quotes (~2%) drop-then-UW-restore, typical drop $532, claw-back $227 | Source data the way UWs do. Build the case for LandGlide or equivalent API, starting with 101+ homes. Bake-off underway (Smarty today, price SmartSource). |
| **Commercial / historic / sober-living** | Soft-check and accept/reject | Manual today | Pluribus/Spotlight LLM historic-district signal expected **~October** (on hold until then). Alternative: agent attestation questions in the flow, Brent says agents prefer upfront questions over UW referral. |
| **Premium age modifier (alternative)** | n/a | Instead of fixing every attribute, price the uncertainty: ~10% bump for 100+ homes, reduced as accuracy improves | Actuarial input ("what rate adjustment maintains loss ratio if corrections never happen?"). Admitted states need rate filings, so not a fast follow. |

- If the first three land, expect rubber-stamp to go from 46.5% toward ~80% (the ~80% is a projection, not a measured number).

## 6. Why NOCs and NOEs are the sensitive weights (Darren + LaNae)

- **NOC and NOE are both bad experiences, and jointly what we are trying to mitigate.** The deal changes after it closed: a NOC cancels the policy, a corrective endorsement takes coverage away (and the legally required NOE letter announces it). NOC is the more severe, but they sit in the same bait-and-switch category and both count on the cost side of the weighing machine.
- **Darren's early data: 3 NOCs = you lose the agent permanently** (not just the policy). First NOC that proceeds to cancellation comes with **~20% drop in future buys from that agent**. Early data points, not published figures.
- **LaNae's two reasons NOCs sting:** the bait-and-switch perception (the deal changed after bind) and the remediation burden (legally required to specify exactly what must be fixed).
- UAR was tried as a pre-cancellation softener and abandoned: agents read it as a cancellation anyway.
- We know the three-strike rule of thumb but not the full underlying mechanism. The scratch-darren curves (74% of NOCs cure; cured NOCs cost ~0 to -2% per event vs. cancellations at -20%, most of it landing right away) are the start of the evidence.
- The optimistic logic: if we do the UW's jobs really well, **the NOC rate should not change** relative to today. The same interventions happen, just automated before bind. Leftover NOCs exist today with a UW in every single 101+ journey, so they are a standing optimization target **whether or not the alert lives.**

## 6b. What dwelling-age NOCs and NOEs are actually FOR

**How to read the table:** every number is *NOCs per 100 bound policies within 90 days of bind* — so 4.52 means 4.5% of policies got a NOC. Each row splits that total by cancellation reason, and the rows add up to the totals. Raw counts behind the rates: 192 NOCs among 4,244 reviewed 101+ policies; 79 among 1,223 twin policies. The Gap column = the extra NOCs per 100 you'd expect from removing the review with nothing in its place.

| Reason | Reviewed 101+ (per 100) | Unreviewed twin 91-100 (per 100) | Gap (removal cost) |
|---|---|---|---|
| Condition - General | 2.71 (60% of NOCs) | 2.86 (44%) | +0.15 |
| Liability Hazard | 0.90 | 1.14 | +0.24 |
| Condition - Roof | 0.28 | 0.98 | **+0.70** |
| Ineligible Risk | 0.49 | 0.90 | **+0.41** |
| Misrepresentation / Pricing | 0.05 | 0.25 | +0.20 |
| Other (named insured, business on premises, unable to inspect) | 0.09 | 0.33 | +0.24 |
| **Total** | **4.52** | **6.46** | **+1.94** |

What this says:

- **The biggest NOC bucket (~60% on reviewed) is Condition - General**: physical-condition findings from the post-bind inspection (deterioration, debris, etc.). The UW review barely moves it (2.71 vs 2.86). No amount of data automation before bind prevents these; that is the NOC floor that will not budge, and the review was never protecting against it.
- **~3/4 of the extra NOCs from removal (1.48 of the 1.94/100 gap) sit in categories the three levers directly address**: Condition-Roof (+0.70 → the roof-exclusion dial), Ineligible Risk + business-on-premises + named-insured (+0.58 → the soft checks / attestations), Misrepresentation (+0.20 → data accuracy). The review's actual protective value is concentrated exactly where the automation plan already points.
- Liability Hazard (~20% of NOCs) is mostly inspection-found and not review-preventable either (0.90 vs 1.14).

**What the NOEs are made of (UW-corrective endorsements, within 90 days):** ~90% are exclusion changes, and **the roof-surfacing exclusion alone is 53% (reviewed book) / 59% (twin) of all UW NOEs**. Attribute corrections are only ~10% (mostly property_type). So the #1 NOE cause after bind IS the roof-exclusion job: widening the dial attacks the biggest NOE lane directly.

Caveats: some cells rest on very few events (the twin's roof row = 12 events), 90-day window only, and the §7 warning still applies: the alert also scares off bad risks, so these rates are a floor.

**Why the review misses the leftover NOCs:**

- **The leftover NOCs come from the post-bind inspection.** The UW review is a desk review done before bind; nobody inspects the property before bind. The exterior inspection completes ~day 21 after bind and surfaces conditions no data source we had before bind carried.
- The NOC'd policies typically scored **125 deficiency points** on inspection (63% scored over 100) vs. a typical 10 for the clean book. **Zero NOCs occur before day 21**; 184 of 192 NOC'd policies had a completed inspection strictly before the cancellation (typical gap 37 days, the UAR response window).
- **No rubber-stamp fingerprint.** The approvals that later blew up were edited by the UW just as often (18.7% vs 24.5%) and got the same decision types as the ones that stayed clean. Editing did not prevent blow-up. The UW isn't missing something visible; the desk review has no eyes on the property.
- **Exception that proves the dial lever:** of the 12 Condition-Roof NOCs on the reviewed book, 0 of 11 checkable had the roof exclusion at bind. The roof job was skipped on exactly these, and it bit.
- The leftover NOEs follow a simple rule: **the worse the inspection finding, the harsher the action.** Moderate scores (80-100) get a corrective endorsement within ~2 weeks of inspection (trampoline 25, private structures 30, roof ACV (actual cash value) 11, animal liability 4 — classic exterior findings); severe scores (125+) get the UAR/NOC track.
- Implication: shrinking the leftover NOC rate means **getting condition information before bind** (imagery at quote time, attestation questions for trampolines/animals/structures) or accepting it as the cost of writing old homes. Either way it's a different problem than the alert.

## 7. Missing dots and risks (the honest list)

- **The alert also scares off bad risks (the "selection effect").** Our projection assumes the same kinds of quotes show up after removal. They may not: the alert deters bad risks from ever submitting. So the NOC projection is a **floor**, not an estimate.
- **The 45% do-nothing is not free.** Part of its value is deterrence, and the catch rate on policies the UW changed is real (6.8% vs. the usual 3.3%). Price it, don't dismiss it.
- **Loss ratio is slow to show up.** Counts (binds, NOCs, NOEs, cancellations) move first, which is why the machine keeps score in counts.
- **Terminology is unresolved.** NOC vs. NOE vs. UAR get used loosely (an NOE rate can exceed the NOC rate, they are separate letters). Sync definitions with LaNae before reporting numbers. All three are letters, and all arrive after bind.
- **4.1% vs 2% voluntary referral rate (just-under cohort): partially resolved.** Both are real cuts of the same rate — 4.1% (Two Houses, YTD window) vs ~2% (8/13 twin cut, Jan-May). Which is right is still open (the two cuts used different time windows and filters), but note: if the clean book runs ~2%, the just-under cohort referring at 4.1% is twice the normal rate, which STRENGTHENS the "agents self-refer when genuinely worried" argument.
- **Reference points to reuse:** the normal NOC/NOE rate climbs ~+22.8% for every 10 years of dwelling age (starting point for 30-60 year homes: NOC 2.75 / NOE 5.11 per 100). Foregone premium from un-run UW edits ~$3,034 per 100 bound, ~0% recovered at renewal.

## 8. The playbook: order of operations for any alert

The repeatable loop once the weighing machine exists. Dwelling age is the first pass; every later alert reruns the same steps cheaper.

1. **Set the tolerance once (step zero).** LaNae/Darren agree what price is acceptable, in a named unit: uncured cancellations per 100 binds, 90 days. Without this, the machine outputs a number and an argument instead of a verdict.
2. **Pick the alert.** By volume of forced reviews. Dwelling age is #1 (5,044 UW touches/month).
3. **Weigh it.** Find the sister cohort (the next age band over, or the era before the alert existed), run the machine: +binds, +NOCs, +NOEs, premium and labor changes, with ranges. If the price is already under tolerance, skip straight to the 10% test: some alerts won't need levers at all.
4. **Investigate the UW's jobs on that alert.** Action mix, what they change, what the leftover NOCs are for. (Done for dwelling age: 46.5 / 25.3 / 28.2 / 8.4.)
5. **Systematize the jobs.** Build the levers, turn them on.
6. **Prove it with the alert still ON.** Zero risk: the gate still catches everything. Two gauges, not one: (a) rubber-stamp % climbs (46.5% toward 80-90%) — the system is doing the jobs; (b) UW override rate on the system's actions stays low — they aren't undoing what it did. Fewer touches plus no corrections of the robot = faithful duplication.
7. **The 10% test.** Turn the alert off for ~10% of traffic. Verify the system rejects/excludes in the right scenarios (the eligibility lane matters most — 82% of real rejections live there) and NOC/fix rates on the test slice hold at reviewed-book levels. Kill criteria pre-agreed. Timing reality: NOCs need ~90 days after bind to read, so how long the test runs is a volume calculation the machine can do.
8. **Re-weigh with real data, then ramp.** The 10% test replaces the sister-cohort estimate: measured, not projected. Present vs tolerance; ramp 10 → 50 → 100.
9. **Log it and loop.** Keep watching the leftover NOC rate; pick the next alert. The machine and levers are reusable infrastructure (the roof dial and pre-fill sourcing help every old-home alert, not just this one).

Why this replaces the 7-state 50/50 test: "pick seven states and eat the NOCs" had no team appetite — it discovers the risk by paying it. The playbook sizes risk on paper (step 3), proves duplication at zero risk (step 6), and only then exposes a small slice (step 7). What survives from Julie's advice is the framing: don't ask underwriting yes/no, help them size the risk, and route the decision to Darren or Christine. Reference if a bigger test ever revives: 50/50 in the top 7 states (bind-to-NOC above 8.8:1 for 90-100yr homes), ~30-60 days.

## 9. Open questions

- **The gate:** what NOC increase will LaNae accept? Never asked directly.
- What is one bind worth vs. one NOC, in dollars? (Bind LTV, NOC cost, agent loss = the Curry model inputs.)
- Does Texas (longest roof-model bake) already show the manual 15% roof-exclusion rate dropping?
- Once the levers land and rubber-stamp hits ~80%, does the machine say kill? Run it, don't assume.
- Which alert goes second?

## 10. Next steps

- [ ] **Thursday 8/20: proposal for the underwriting sync.** Dwelling age as weighing machine v1: bind upside, bounded downside, three levers as mitigants. Framed as risk-sizing, not a yes/no ask. Decision routes to Darren/Christine.
- [ ] **Ask LaNae her tolerance number** (or make it the explicit ask inside the Thursday proposal).
- [ ] Meet David Curry: scope the bind/NOC model (bind LTV, NOC cost, UW premium per alert).
- [ ] With Curry: expand roof-exclusion threshold to bottom 20% for older dwellings.
  Scoped 8/20: `dwelling-alert/roof-dial/SCOPE.md` (data verified, sweep tool ready; bound 101+ book is 94.5% hand-applied).
- [ ] Check Texas NOC/NOE data for roof-model impact.
- [ ] Build the LandGlide (or equivalent) pre-fill sourcing case, starting with 101+ homes; get SmartSource pricing.
- [ ] Explore the premium age modifier with actuarial.
- [ ] Review the conditional liability exclusion form (eng + comms lift).
- [ ] Validate Darren's three-NOC rule when his final figures land.

---

## Terminology

- UAR, NOC, and NOE are all LETTERS, and all post-bind. UAR = Underwriting Action Required, a warning letter with a response deadline (~1,270/mo). NOC = Notice of Cancellation (~1,570/mo). NOE = Notice of Endorsement, the legally required letter announcing an adverse coverage change UW already made (only ~350/mo company-wide).
- One workflow drives UAR and NOC (modes uar / uarnoc / noc). 73% of NOCs have a UAR parent, 27% are issued directly, and 93% of UARs still end in a NOC.
- **Shorthand in THIS doc:** "NOC" = a UW cancellation event under the cancellation_reason='Inspection' umbrella (14 reason types collapse into that bucket; the name does not mean inspection-caused), counted even if the policy later cures, unless stated otherwise. "NOE" = a UW corrective endorsement TRANSACTION (attribute correction or exclusion change by a UW actor), roughly 45x the volume of the legally required letters, so never compare our counts to letter counts.
- A NOC issued is not a policy lost: **74% of NOCs cure**, and 23.3% of inspection-NOC policies reinstate within 90 days. Every NOC number must state its stage: letters/events (all of them, cured or not) or cancellations that stuck (uncured).
- Bare "NOC" elsewhere in the company includes non-payment cancellations. Always say "underwriting" when quoting a rate.
- Salesforce cannot referee any of this: the 'UAR/NOC Approval' category cannot separate a UAR from a NOC, over-counts by ~20%, and misses ~50% of inspection-NOC policies. Counts come from the eventstore (the system's raw log of every action).
- Darren's metrics, precisely: his agent-loss result keys to the first UNCURED cancellation (~-20% of that agent's production for a year, most of it landing early); cured NOCs price at ~0 to -2% per event. The "3 NOCs = agent gone" soundbite never says 3 NOCs out of what, over what period; park it until he names the unit.

### Definitions to lock with LaNae before quoting numbers Thursday

- **Name the unit of her tolerance.** "NOC increase I'd accept" can mean letters sent, policies cancelled, or cancelled net of cure — at 74% cure those differ ~4x. Proposal: set the gate on uncured cancellations per 100 binds, within 90 days.
- **Decide how a cured NOC counts.** Her two stated pains (bait-and-switch perception, remediation burden) attach to the LETTER, which lands even when the policy cures. If that's the real pain, the tolerance needs two numbers: a letter budget and an uncured budget.
- **Pre-agree the NOE relabel** (transactions vs the ~350/mo legally required letters) so her team doesn't reconcile us against letter volume and find us 45x off.
- **Confirm NOC scope**: ours = UW cancellations only, all 14 reasons, no non-pay. Darren's book-wide number (every NOC, cured or not) is 11.7% of new binds within 90 days; the twin runs 6.5/100; our reviewed 101+ book runs 4.5.
- **Fix the clock**: everything is per 100 bound, 90-day window (typical bind-to-NOC gap 44 days, ~90% inside 90).

## Appendix: sources

Published artifacts (mine):

- [What Must Be True](https://claude.ai/code/artifact/17b10280-3546-453c-8e9a-b6ee95c16ae8) — the weighing machine framing, v4 (8/18). Source HTML copied to [appendix/what-must-be-true.html](appendix/what-must-be-true.html).
- [Two Houses](https://claude.ai/code/artifact/443f560b-9bf3-4e43-be04-27f79d015393) — dwelling age baseline; the +22.8%/decade ramp is in §2.
- [The Dwelling Age Plan](https://claude.ai/code/artifact/b2948c69-3b9e-4eaf-a0d5-01ea005152fe) — the 8/17 project plan.
- [Why Old Roofs Slip Through](https://claude.ai/code/artifact/cc9e164c-8f41-40d1-8d02-99e3b152a43e) — the roof exclusion dial gap (3.5% vs 20%).
- [DAMR — pre-fill flip validation](https://claude.ai/code/artifact/6b9d96a2-67b1-496f-9f08-ce2d9fbda1b6) — agent-touched values corrected 2-6x more (8/7).
- [The Pre-fill Bake-off](https://claude.ai/code/artifact/f743c2ea-d0a3-4555-8c6b-41eddcaf69b5) — provider comparison design (8/17).
- [Water Automation — Bind-Rate Impact](https://claude.ai/code/artifact/3f60f61a-020f-4464-b864-3ce4c172aaef) — the lived proof that automation alone adds zero binds (the weighing machine's backtest case).
- [Excessive-Claims Automation — Live Impact](https://claude.ai/code/artifact/693290c3-65a0-47a5-8f8a-6acecc728e18) — the claims-alert automation program this playbook generalizes.
- [Claims Alerts Walkthrough — Agent & Underwriter (Prototype)](https://claude.ai/code/artifact/63c4f2b7-1e80-487d-8ea9-6fee25db0c8c) — what the alert experience looks like on both sides of the screen.

Shared with me:

- [Dwelling-Age Pre-Bind Review — Findings (Aug 2026)](https://claude.ai/code/artifact/cd621252-2b1d-4e3e-a088-4b55592d24be)

Other sources:

- [scratch-darren NOC impact report](https://github.com/landlordhq/scratch-darren/blob/main/2026-08-noc-impact-inde-agent/REPORT.md) — cure rates, retention curves, `tradeoff.py`.
- [appendix/damr_prefill_flip_briefing_v8.pdf](appendix/damr_prefill_flip_briefing_v8.pdf)
- [appendix/damr-end-to-end-briefing.pdf](appendix/damr-end-to-end-briefing.pdf)

Meetings (Granola):

- LaNae / Suph, Aug 18 — NOC pain, Darren's data, Curry weighing-machine plan
- Suph / Brent, Aug 18 — AFI benchmark, attestation-questions idea
- David / Suph, DAMR Alert Test, Aug 17 — bind lift proven, tolerance-first sequencing
- Julie / Suph, Aug 17 — 50/50 test design, risk-sizing framing, age modifier
- Suph // Christine 1:1, Aug 13 — zero-alerts north star, upstream pre-fill thesis
- Product / UW Sync, Aug 6 — Pluribus/Spotlight October timeline

Local working folders (this laptop): `~/mockups/alert-north-star/`, `~/mockups/dwelling-age-project-plan/`, `~/mockups/damr-*/`.
