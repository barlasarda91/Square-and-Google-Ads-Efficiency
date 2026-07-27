# 10 - DEMAND AND OPS (Square). Runs every time.

This is the scoreboard. It answers one question: **is the business growing, and is the afternoon
moving.**

## Pull

Use the Square Reporting API `load` on the **Sales** view. Request aggregated rows, never raw
orders. Boxx does 200+ transactions per day and the orders endpoint caps at 1,000 results per
page, so order-level aggregation is impractical and will time out.

- Measure: `Sales.order_count`
- Dimension: `Sales.local_date`
- Also pull `Sales.net_sales` and `Sales.gross_sales` by the same dimension for the margin proxy
- Filter: `Sales.local_hour` with **string** values
- Single-location merchant: **do not add a location filter**

Three dayparts, last **14 days** each:

| Daypart | local_hour | Meaning |
|---|---|---|
| Daytime | gte 7, lte 17 | 7AM to 6PM |
| **Afternoon** | gte 14, lte 17 | **2PM to 6PM. The headline.** |
| Night | gte 18, lte 22 | 6PM to 11PM, Thu/Fri/Sat only |

## Validate before analyzing

Take one settled date that already exists in `data/square_daily.csv` and re-pull it. If the figure
differs, **stop analyzing** and report a data integrity problem: state the date, the stored value,
the re-pulled value, and the query you used. Do not overwrite the stored value. A silent drift
between runs is more dangerous than a missed week.

## Append to the history file

Write one row per new date into `data/square_daily.csv`. Never rewrite existing rows. Tag any date
inside a contamination window.

## Analyze

1. **Afternoon against the ladder.** Trailing 14-day 2-6PM average. Current rung, distance to next
   rung in tx/day, direction of travel across the last three runs. Lead with this.
2. **Margin guardrail.** Net sales per transaction for the 2-6PM window against the trailing
   four-week non-promo weekday figure. If down more than 8%, this becomes the lead finding, ahead
   of transaction count.
3. **Daytime.** Trailing 7-day average, and **day-of-week matched** week-over-week: Tuesday against
   Tuesday, Saturday against Saturday. Never compare raw seven-day means across different weekend
   mixes. Report the day-matched delta, not the raw one.
4. **Best and worst day** in the window, with the likely reason if one is available.
5. **Night at Boxx.** Transactions per active night against the 50 target. Night is funded at
   $1/day and is deliberately divested. Report it, but do not recommend re-funding it without an
   explicit strategy decision from Arda.
6. **Contamination.** Read every number against `CONTAMINATION_LOG.md`. If you spot a new event in
   the data, an unexplained single-day collapse or spike, append it to the log with your best
   hypothesis clearly marked as a hypothesis.
7. **Open recommendations.** For each item in `state.json > open_recommendations`, state its age in
   days and whether the data shows any sign it was executed. An open recommendation older than 21
   days gets called out explicitly.

## Spend pull - every run, not just sweep runs

Cost settles within about a day, so there is no reason to let the spend series develop gaps between
biweekly sweeps. On **every** run, pull from Google Ads for the last 14 days, per campaign:

- daily cost, impressions, clicks, CTR, average CPC
- the current daily budget on each active campaign

Append to `data/ads_daily.csv`, one row per date per campaign. Leave the conversion columns empty
on non-sweep runs rather than filling them with unsettled figures. Never rewrite an existing row.

Then check the current budget on each campaign against the last row for that campaign in
`data/budget_changes.csv`. **If a budget, bidding strategy, geo, schedule, or status differs from
what the ledger says it should be, that is a finding, not a housekeeping note.** Append a row to
the change ledger with `source` set to `Google` if you cannot attribute it to a logged Arda
decision, and put it near the top of the output. Google has made unauthorised changes to this
account before and roughly $914 went out before anyone noticed.

If Google Ads is unreachable on a non-sweep run, continue with Square, note the spend gap
explicitly, and leave those dates absent from `ads_daily.csv` so the next run backfills them.

## Spend against revenue

Using `data/ads_daily.csv`, `data/budget_changes.csv`, and `data/square_daily.csv`, report:

1. **Last 14 days**: total spend, spend per day, budgeted per day, and the delivery ratio. Call out
   any campaign delivering above 110% or below 80% of budget.
2. **Ad cost as a percent of net sales** for the same window, and the same figure program to date.
3. **The regime table** from `reference/SPEND_LEDGER.md`. Add the current regime as an in-progress
   row. Mark every regime `clean` or `contaminated` against `CONTAMINATION_LOG.md`.
4. **Program to date**: cumulative spend since 2026-03-21, and cumulative by campaign including
   paused and divested ones.

Print the caveat from `SPEND_LEDGER.md` in the same block as the regime table, every time. A
marginal cost per transaction from a before-and-after comparison is not incrementality, and this
account has already shown why: spend went to zero for six days in June and daytime transactions
barely moved.

## Google as support only

One line, using settled data from the last sweep: is paid capture tracking with Square or diverging.
No conversion analysis on a non-sweep run.

## What not to do

- Do not compute a lift figure across the Jul 16 budget reallocation date without labelling it.
- Do not attribute a change to advertising when the ads-on and ads-off averages are within noise.
  The established finding is that daytime is inelastic to ad spend at current levels.
- Do not recommend anything you cannot tie to a number you pulled this run.
