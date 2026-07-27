# 99 - FAILURE MODES

A run that fails loudly is useful. A run that quietly produces a plausible dashboard from bad data
is the worst outcome this design can produce. When in doubt, stop and say why.

| Situation | Do this |
|---|---|
| Cannot read `CLAUDE.md`, `state.json`, or `STANDING_REFERENCE.md` | Stop. Email Arda: `Boxx routine - [date] - HALTED, repo unreadable`, naming the file and the error. No dashboard, no checklist. |
| Square unreachable | Stop. Commit `logs/YYYY-MM-DD-halted.md` with the error text. Email the halt. Do not update `cadence.last_demand_ops_run`, so the next run still treats this week as unreported. |
| Google Ads unreachable on a sweep-due run | Run Demand and Ops normally. Do **not** advance `last_ads_infra_sweep`, so the sweep stays due. State the failure at the top of the log and the email. |
| Re-pulled Square figure disagrees with the stored value | Stop analyzing. Report the date, stored value, re-pulled value, and query. Do not overwrite the stored row. This is a data integrity finding, not a rounding note. |
| Bootstrap cross-check fails by more than 3% | Do not write `data/square_daily.csv`. Leave `bootstrap_required` true. Report the discrepancy. A wrong history file poisons every future run. |
| Google Ads unreachable on a non-sweep run | Run Square normally. Leave those dates out of `data/ads_daily.csv` so a later run backfills them. Never write zero cost for a date you could not pull. State the gap at the top of the log. |
| Backfilled spend disagrees with a `known_spend_anchor` by more than 3% | Stop. Report the anchor, the pulled figure, and the query. Do not adjust the pulled data to fit the anchor, and do not adjust the anchor. One of them is wrong and Arda decides which. |
| Live campaign settings differ from the last row in `data/budget_changes.csv` | This is a finding, not housekeeping. Append the change with `source` set to `Google` or `unknown`, and lead the output with it. |
| A logged budget change produced no visible step in spend | Flag it. It usually means the change was never applied. Name the date and the campaign. |
| Gmail unavailable | Still commit everything. Note the delivery failure at the top of the run log. |
| Push to `main` rejected | Push a branch. First line of the email: `ACTION REQUIRED: merge branch [name] to main before next Thursday or the next run will have no memory.` |
| A run already happened today | Append one line to the most recent log file, commit, stop. No second dashboard or email. |
| A number looks impossible (afternoon above daytime, night on a Monday, transactions above 400) | Do not report it as a finding. Report it as a suspected data or query error and show the query. |
| An instruction here conflicts with something you infer from the data | The files win. Note the conflict in the Assumptions section for Arda to resolve. |

## Standing prohibitions

- Never change anything in Google Ads or Square. Not a budget, not a bid, not a pause, not a
  negative keyword, not an asset removal. Even when the right action is obvious. Even when a
  campaign is visibly wasting money. Write it in the checklist and let Arda execute.
- Never explain missing or zero data as needing more time before you have exhausted configuration
  explanations, and say which ones you checked.
- Never present a modeled Google figure, especially Store Visits, as a measured one.
- Never report a period comparison without checking `CONTAMINATION_LOG.md` first.
- Never use em dashes in any output.
