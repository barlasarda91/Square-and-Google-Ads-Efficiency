# Boxx Routine - 2026-07-17 (baseline, written by hand)

Not a routine run. This is the seed entry so the first scheduled run has a prior state to compare
against. Figures come from the Jul 16 decision session and Square Reporting API pulls through
Jul 15, 2026.

## Summary

1. Afternoon 2 to 6PM: about 51 per day. Rung 0. 19 per day to rung 1. Trend flat.
2. Daytime: 204.0 per day for Jul 13 to 15, a partial week. Trailing clean weeks sit low 200s to 220s.
3. Best recent day is Saturday, running 256 to 268. Worst is Jul 4 at 155.
4. Night at Boxx best night Jul 3 at 18. Range 6 to 18 per active night against a 50 target.
5. Headline: daytime is inelastic to ad spend. Ads-on 215.8 against ads-off 218.8. The gap to 270
   lives in weekday afternoons, not in the ad account.

## Data windows

Square: Jun 5 to Jul 15, 2026, daily, 7AM to 6PM daytime, 6PM to 11PM night.
Google: Jun 12 to Jul 6, 2026, settled. No provisional tail carried forward.

## Backfill and restatement

None. No prior routine run exists.

## Square scoreboard

Weekly daytime averages: May 28 to Jun 1 = 242.2 (peak, contains Memorial Day). Jun 5 to 10 =
232.7 (suspension floor). Jun 11 to 14 = 237.2 (bounce). Jun 15 to 21 = 171.3 (reduced hours,
exclude). Jun 22 to 28 = 213.6. Jun 29 to Jul 5 = 204.1 (contains Jul 4). Jul 6 to 12 = 223.7
(viral tail). Jul 13 to 15 = 204.0.

Daypart: mornings at bar capacity, 25 to 31 orders per hour 8 to 10AM. Afternoon 2 to 6PM about 51
orders total, 5PM at 4.7 and 6PM at 0.9.

## Google read

Campaign 1 Search: $1,419, 26.1K impressions, CPA $2.47, flagged **Limited by search volume**.
"coffee near me" took $860, 60% of campaign spend, with zero tracked conversions.
PMax trial: $563, 181.5K impressions, display-heavy, 765 local actions, 409 modeled store visits.
Night: $412, 14 conversions, about $29 CPA.
Geo: Campaign 1 concentrated DTLA, 80% of clicks. PMax drifting north toward ZIP 90012.

## Trigger check

TRIGGER CHECK - 2026-07-16: 0 of 12 reached. Triggered: none. Not yet: T3, T12. Superseded: T1, T2.
Divested: T11. Contraindicated: T9. Held: T8. Blocked: T4, T5, T6, T7, T10.

## Decisions applied Jul 16

Campaign 1 $45 to $20 per day and narrowed to Downtown LA only, Boyle Heights dropped.
Night at Boxx $15 to $1 per day, kept enabled.
PMax trial $20 to $40 per day, phased toward $60.

## Correction on record

An earlier claim that the website tag reinstall was the top priority for PMax measurement was
wrong. The store-visit PMax optimizes toward Store Visits and Get Directions, both measured
through Google location and GBP signals, not the website tag. The tag matters only for DTC
purchase tracking and remarketing audiences. The real verification path for in-store sales is a
Store Sales offline conversion import of Square transactions, which is a project, not a task.

## Open recommendations at baseline

See `state.json > open_recommendations`. Five open, one overdue (Square lapsed Customer Match list
refresh, was due Jul 21).

## Assumptions

Afternoon baseline of 51 is taken from the Jul 16 daypart diagnosis rather than from a daily
series. The first scheduled run should replace it with a measured trailing 14-day figure during
bootstrap.

## Spend ledger at baseline

Ledgers were seeded by hand on 2026-07-27 from the session logs. `data/ads_daily.csv` is empty and
is filled by the first run's bootstrap backfill, 130 days back to program start 2026-03-21.
`data/budget_changes.csv` holds 22 change points, four of them marked `inferred` because the value
is known but the exact change date is not.

Current budgeted daily total is $71.00: Campaign 1 at $20, PMax trial at $40 pending confirmation,
DTC Shopping at $10, Night at $1.

Known spend anchors for cross-checking the backfill are in `state.json > spend_ledger`. Cumulative
program spend has never been computed. The first run produces it.
