# 20 - ADS AND INFRA SWEEP (Google Ads). Only when due.

Apply the settled-window rule from `00_preflight.md` throughout. Every conversion figure in this
section is settled-window only. Cost, clicks, impressions, and CTR may use a more recent window,
labelled separately.

## Per active campaign

Campaigns: `Boxx Store Visits - 1` (Search), `Local Store Visit and Promotion - Google Template
Based` (the PMax trial), `Boxx DTC - Coffee - Shopping - LA` (Shopping), `Night at Boxx - 1`
(Search, $1/day). Ignore `Boxx Store Visits - 2 - Performance Max`. It is paused permanently.

Pull:

- Daily impressions, clicks, CTR, average CPC, cost.
- Conversions **segmented by conversion action** (Hedefler, Donusumler). For PMax specifically,
  split Get Directions from modeled Store Visits. Do not report a combined conversion number for
  PMax without that split, it is the single most misleading figure in this account.
- Cumulative and trailing-30-day settled Get Directions.
- The **backfill delta** against last run's settled figures, stated explicitly.
- Search terms report. Quantify competitor leakage as a share of campaign spend. "coffee near me"
  was 60% of Campaign 1 spend at $860 with zero tracked conversions as of Jul 16.
- Geographic distribution. Campaign 1 should be Downtown LA only since Jul 16. PMax was drifting
  north toward ZIP 90012. Flag any drift.
- Assets filtered to "Added by: Google AI" (Araclar, Reklam Ogeleri Studyosu). **List them and
  recommend removal. Do not remove them.**
- Serving flags: "Limited by budget" versus "Limited by search volume." These mean opposite things.
  Search-volume-limited means more budget buys nothing.
- Change history (Degisiklik gecmisi) since the last sweep. Anything you did not expect, especially
  Google-initiated changes, is a finding.

## Ground it

Pull trailing 28-day Square daytime alongside. State plainly whether ad spend and Square
transactions moved together or independently over that window.

## Complete the ledgers

The daily cost rows were already appended by `10_demand_ops.md`. On a sweep run, go back and fill
the conversion columns in `data/ads_daily.csv` for every date now inside the settled window, using
`restated_from` to record what the figure was before it settled. This is how we learn the real
lag rather than assuming it.

Then rebuild the regime table per `reference/SPEND_LEDGER.md`, close out the regime that ended at
the last change point, and report:

- cost per marginal transaction against the prior clean regime, with the caveat attached
- ad cost as a percent of net sales, this regime and program to date
- cumulative spend by campaign since 2026-03-21, including paused and divested campaigns
- delivery ratio per campaign, actual spend against budgeted

Any campaign whose current settings differ from the last row in `data/budget_changes.csv` is a
finding. Append the change with `source` set to `Google` where you cannot attribute it, and lead
the sweep output with it.

## Trigger check

Evaluate all 12 triggers from `reference/TRIGGERS.md` on settled data only. Report in exactly
this format:

`TRIGGER CHECK - [date]: X of 12 reached. Triggered: [...]. Not yet: [...].`

For each newly met trigger: exact recommended action, rationale, expected impact. Then stop.
Respect every permanent Phase 1 decision. Carry forward every SUPERSEDED, DIVESTED, and
CONTRAINDICATED status from the file rather than re-deriving it.

Also evaluate the three new-trigger candidates at the bottom of `TRIGGERS.md`: afternoon rung met,
margin guardrail breach, PMax step validation.

If nothing fired and nothing is broken, say so in one line. Do not manufacture recommendations.

## Blocker status

Update each item in `state.json > known_blockers` with anything you learned: the website tag, the
Shopify Customer Match consolidation, the conversion value session, and the offline Store Sales
import. The Store Sales import is the only genuine path to verifying in-store sales against ad
spend and should be described as a project, not a task.

## PMax step validation

The PMax trial is stepping $20 to $40 to $60. After each step, check whether day-matched Square
daytime moved by at least the lift implied by the previous step. If it did not, recommend holding
the next step and say exactly what evidence would justify resuming.
