# Dwelling alert

The project: get the system to duplicate what underwriters actually do on the dwelling age alert, prove the duplicate holds, then remove the alert. The evidence for why this works lives one level up (`../README.md`, sections 5 and 6b): the underwriter's protection is concentrated in a small set of repeatable jobs.

The underwriter does four jobs on an old-home referral:

1. **Apply the roof surfacing exclusion** (the biggest post-bind lane: this one exclusion is 53 to 59% of all UW corrective NOEs)
2. **Correct property data** (six fields carry ~90% of the corrections)
3. **Run eligibility soft checks** (ineligible risk, business on premises, named insured)
4. **Approve** (the default outcome; ~65% of referrals end in plain approval)

Each job gets its own folder here as we scope and build it. Duplicate all four, and the alert stops earning its keep.

## Folders

- `roof-dial/` — **job 1, scoped 2026-08-20.** Where should the robot's score bar sit so it captures most of the exclusions underwriters hand-apply, without excluding roofs an underwriter would have left alone? Scope, verified data inventory, extract SQL, and a runnable sweep tool. Status: data verified, analysis pending.
- (jobs 2 to 4: not yet scoped; the pre-fill bake-off covers part of job 2)

## Relation to the rest of the repo

- `../README.md` — the thesis, the evidence, and the 9-step playbook this project follows
- `../weighing-machine/` — a separate tool that prices whole alerts for removal; once the roof dial analysis lands, its results become inputs there (catches turn into prevented NOEs and NOCs). This folder does not depend on it.
