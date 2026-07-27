# 00 - PREFLIGHT

## 1. Establish the date

`RUN_DATE` = today, America/Los_Angeles. All windows are Pacific. Every window you report carries
explicit start and end dates. Never write "last week" without the dates beside it.

## 2. Verify connectors

Confirm you can reach:

- **Square** - required every run. No Square, no run.
- **Google Ads** - required every run for the daily cost pull. Required and load-bearing on
  sweep-due runs, where conversions and search terms are also pulled.
- **Gmail** - required for delivery.

If Square is unreachable, or Google Ads is unreachable on a sweep-due run, go to
`99_failure_modes.md`. Do not analyze on partial data and do not silently degrade. If Google Ads is
unreachable on a non-sweep run, continue with Square, leave the missing dates out of
`data/ads_daily.csv` so a later run backfills them, and say so at the top of the output.

## 3. Decide what is due

Read `state.json`.

- **Bootstrap.** If `bootstrap_required` is `true`, run the bootstrap in section 5 before anything
  else, then set it to `false`.
- **Demand and Ops** (`10_demand_ops.md`) runs **every time**. No condition.
- **Ads and Infra** (`20_ads_infra.md`) runs if
  `RUN_DATE - cadence.last_ads_infra_sweep >= cadence.ads_sweep_interval_days`, or if
  `last_ads_infra_sweep` is null. Otherwise write `Ads and Infra sweep not due, next due [date]`
  in the output and skip it.
- **Double-fire guard.** If `cadence.last_demand_ops_run` equals `RUN_DATE`, a run already
  happened today. Append a one-line note to the most recent file in `logs/`, commit, and stop.
  Do not produce a second dashboard or email.

## 4. Load context

Read, in full: `reference/STANDING_REFERENCE.md`, `reference/TRIGGERS.md`, `reference/LADDER.md`,
`reference/CONTAMINATION_LOG.md`, the last two files in `logs/`, and `data/square_daily.csv`.

Note every item in `state.json > open_recommendations`. You will report on each one's age and
whether the data suggests it was acted on.

## 5. Bootstrap (first run only)

The daily history files are the foundation of every comparison this routine will ever make.
Populate them once. `bootstrap_backfill_days` is 130, which reaches back to the program start on
2026-03-21.

**Revenue side:**

1. Pull `bootstrap_backfill_days` days of daily Square data (see `10_demand_ops.md` for the exact
   query shape) for all three dayparts.
2. Write one row per date into `data/square_daily.csv` using the header already in that file.
3. Cross-check three dates against `reference/CONTAMINATION_LOG.md` weekly averages. If your
   pulled figures disagree with a logged average by more than 3%, **stop** and report a data
   integrity problem instead of writing the file. A wrong history file poisons every future run.
4. Tag any date falling inside a contamination window with the matching `type` in the
   `contamination` column.

**Spend side:**

5. Pull the same date range of daily Google Ads cost, impressions, clicks, CTR, and average CPC,
   **per campaign, including paused and divested campaigns**. Campaign 5 and the divested Night
   campaign both spent real money and belong in the history.
6. Write one row per date per campaign into `data/ads_daily.csv`. Leave conversion columns empty
   for dates you cannot pull settled conversion data for. Empty is honest, zero is a lie.
7. Cross-check against every entry in `state.json > spend_ledger.known_spend_anchors`. These are
   figures taken from session logs. If a backfilled window disagrees with an anchor by more than
   3%, **stop** and report it. Do not reconcile it yourself by adjusting the pulled data.
8. Verify `data/budget_changes.csv` against what the spend series actually shows. If spend steps
   at a date with no corresponding change-ledger row, append one with `source` set to `unknown`
   and `confidence` set to `inferred`, and flag it in the run log. If a logged change produces no
   visible step in spend, flag that too, it usually means the change was never applied.
9. Compute and write the cumulative fields in `state.json > spend_ledger`.

Then set `bootstrap_required` to `false` and note both backfill ranges in the run log.

## 6. The settled-window rule

Google Ads does not register conversions in real time. Get Directions and Local Actions attribute
back to the click date and keep climbing for about three days. Store Visits are modeled and settle
over weeks, and never reconcile to Square.

- **Settled window**: dates on or before `RUN_DATE - 3`. All conversion analysis and every trigger
  check use this window only.
- **Provisional tail**: `RUN_DATE - 2` through `RUN_DATE`. Report separately, labelled
  "provisional, still settling." Never draw a conclusion from it.
- **Backfill check**: re-pull last run's settled window and record how much it moved, for example
  "last run's Get Directions revised 88 to 96." This is how we learn the true lag and how much to
  trust the numbers.
- Cost, clicks, impressions, and CTR settle within about a day and may use a more recent window.
  Keep the two windows separate and labelled.
- Square settles same or next day and needs no lag treatment.
