# 30 - OUTPUTS

Four artifacts, in this order. Do not skip the commit.

## 1. Run log - `logs/YYYY-MM-DD.md`

The full record. Structure:

```
# Boxx Routine - YYYY-MM-DD (Demand and Ops[ + Ads and Infra])

## Summary
Five lines, no more:
1. Afternoon 2-6PM average vs the ladder, current rung, direction
2. Daytime 7-day average and day-matched WoW delta
3. Best and worst day
4. Night best night vs the 50 target
5. The single most important recommendation or flag
[On sweep runs, replace line 5 with: triggers reached and the top action.]

## Data windows
Square range. Google settled window and provisional tail, labelled separately.

## Backfill and restatement
What last run's settled numbers revised to, and by how much.

## Square scoreboard
## Spend against revenue
Last 14 days spend, budgeted, delivery ratio. Ad cost as percent of net sales, window and program
to date. The regime table with the caveat attached. Cumulative spend by campaign. Any drift between
live settings and the change ledger.
## Google read (sweep runs)
## Trigger check (sweep runs)
## Recommended actions
Numbered. Each one: exact change, rationale, expected impact, and how we will know if it worked.
## Contamination log additions
## Assumptions made
Everything you decided without being able to ask.
```

## 2. Dashboard - `dashboard.html` at repo root, plus a dated copy in `logs/`

Fill `templates/dashboard.html`. Replace every `{{PLACEHOLDER}}`. It is a single self-contained
file with no external requests, so it opens from disk on any device.

The signature element is the **ladder gauge**: the trailing 14-day 2-6PM average plotted against
the four rungs. That is the first thing on the page and the largest thing on the page.

Every figure on the dashboard carries its data window. Provisional Google figures are visually
marked as provisional. If a figure is contaminated, it is marked contaminated. A dashboard that
looks confident about a dirty number is worse than no dashboard.

## 3. Action checklist - `checklist.md` at repo root, plus a dated copy in `logs/`

A standalone file, not a section of the log. Fill `templates/checklist.md`. Rules:

- Every item is a single concrete action Arda can execute in one sitting.
- Every item names the exact surface and path, including Turkish menu labels where relevant.
- Every item carries: why, expected impact, and how we will know if it worked.
- Carried-over items keep their original ID and show their age in days.
- Items are ordered by expected impact, not by which system they touch.
- Anything older than 21 days is marked `STALE` and gets one line on whether it is still worth doing.
- Never include an item you would execute yourself. You do not execute anything.

## 4. Email - via Gmail

To Arda. Subject: `Boxx routine - [date] - afternoon [N] of 103, daytime [N]`.

Body: the five-line summary, then the numbered actions, then one line naming where the dashboard
and full log live. Plain text, declarative, no em dashes. Nothing in the email that is not also in
the log.

Send it. Do not leave it as a draft.

## 5. Commit

Update `state.json` first:

- `cadence.last_demand_ops_run` = RUN_DATE
- `cadence.last_ads_infra_sweep` = RUN_DATE if the sweep ran
- `scoreboard_last_known` and `google_settled_last_known` = this run's figures
- `spend_ledger` = cumulative spend to date and by campaign, `cumulative_spend_as_of`,
  ad cost as percent of net sales, current budgets, and `last_regime_change` if a change point
  was found or added this run
- `open_recommendations` = carry forward unresolved items with original IDs, add new ones, mark
  resolved ones `closed` with the date, and do not delete closed items until they are 60 days old
- `known_blockers` = anything learned this run

Then commit and push everything: `state.json`, `data/square_daily.csv`, `data/ads_daily.csv`,
`data/budget_changes.csv`, the new `logs/` files, `dashboard.html`, `checklist.md`, and any edit to
`reference/CONTAMINATION_LOG.md`.

Commit message: `routine: YYYY-MM-DD demand-ops[ + ads-sweep]`

**Push to `main`.** The next run reads its state from `main`. If it lands on a `claude/` branch
instead, the following run starts blind and the entire memory design fails. If the push to `main`
is rejected, that is a permissions problem, not a reason to give up quietly: push the branch, and
make the **first line** of the email `ACTION REQUIRED: merge branch [name] to main before next
Thursday or the next run will have no memory.`
