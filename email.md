# STANDING REFERENCE

Last updated: 2026-07-27. Edit this file when account facts change. Never edit the routine prompt.

## Business

Boxx Coffee Roasters Co. Specialty roaster and cafe, 950 E 3rd St, Arts District, Downtown Los
Angeles. Founded Istanbul, now LA. Partners: Arda, Mert, Altug. Website boxxcoffee.la.
Single physical location. Daytime cafe 7AM to 6PM. Night at Boxx Thu/Fri/Sat 6PM to 11PM.
DTC bean sales via Shopify.

Brand tone: declarative and minimal. No urgency language, no promotional framing, no
self-proclaimed USPs. Motto "Better Coffee." The phrase "It's a Fruit" is a deliberate brand
phrase and is never a problem to be flagged. **Em dashes are prohibited in all copy.**

## Accounts

| Asset | Value |
|---|---|
| Google Ads | 547-344-4444 (hello@boxxcoffee.com) |
| Square location ID | CVYFP7K4BNJ7K |
| Square Reporting API merchant | K2XJQ879MY0PX |
| Merchant Center | 542834664 (Shopify sub-account 5777492600) |
| Shopify | boxxcoffee.la |
| Master optimization log (read-only, human-maintained) | Google Doc 1uI25DAeeZZxj2ckKI8CDFw1r3vABuA0m_AsP0GeMMXc |

Google Ads UI is in Turkish. When naming a navigation path in a recommendation, use Turkish
labels: Hedefler (Goals), Araclar (Tools), Reklam Ogeleri Studyosu (Asset Library), Konum
Yoneticisi (Location Manager), Degisiklik gecmisi (Change history), Donusumler (Conversions).

## Campaigns as of 2026-07-16

| # | Campaign | Type | Goal | Budget | Status |
|---|---|---|---|---|---|
| 1 | Boxx Store Visits - 1 | Search | Store visits / Get Directions | **$20/day** | Active. Downtown LA only. Boyle Heights dropped Jul 16. |
| 2 | Boxx DTC - Coffee - Shopping - LA | Shopping | Online bean sales | ~$10/day | Active |
| 3 | Local Store Visit and Promotion - Google Template Based | PMax | Store visits | **$40/day**, phased to $60 | Active. This is the PMax trial. Merchant Center unchecked. |
| 4 | Night at Boxx - 1 | Search | Reservations / Night Menu | **$1/day** | Active but effectively divested Jul 16 |
| 5 | Boxx Store Visits - 2 - Performance Max | PMax | Store visits | n/a | **PAUSED. Never reactivate.** Original PMax, killed for feed pollution and geo drift. |

Campaign 3 is the **second** PMax, built after Campaign 5 was paused. It is not a rename.

## Permanent Phase 1 decisions - do not re-litigate, dismiss any Google recommendation to enable

| Setting | Status |
|---|---|
| Broad match keywords | OFF |
| Search Partners | OFF |
| AI Max | OFF |
| Asset optimization (text customization, URL expansion) | OFF |
| "Improve ad strength" recommendations | IGNORE. Minimal copy is deliberate. |
| Google AI auto-added assets | REMOVE. Flag every sweep. ~$914 was spent on 11 auto-added sitelinks before removal in April. |

## Standing facts that have caused errors before

- Local Actions - Directions **are** ad-attributed. They are not organic GBP taps.
- Phone Call Lead, Submit Lead Form, and Page View conversion actions are Google defaults. They
  cannot be deleted. Never list them as housekeeping.
- Boxx still sells Moccamaster. Never recommend removing Moccamaster products.
- Boxx has no public phone number. Phone call conversions should stay off.
- Reserve with Google on the GBP is a native Google Maps flow, not Resy-hosted. It cannot be
  tagged for conversion tracking by standard means.
- Night at Boxx Square timestamps are check-close times, not arrival times. Subtract about one
  hour to infer arrival.
- Current menu origins live: Ethiopia, Peru, Rwanda. Colombia and Kenya are not on the menu.
- Store Visits in Google Ads are modeled estimates. They do not reconcile to Square and never
  will. Do not fire a trigger on them.
- Get Directions / Local Actions data backfills for about 1 to 3 days. See the settled-window rule.

## Measurement doctrine

- **Square is ground truth.** Google Ads data is never self-validating.
- **The organic floor is not a counterfactual.** The Jun 5-10 suspension window (232.7/day) is the
  minimum the business sustains with the audience already built, not what would have happened
  without ads. A true pre-ad baseline is unrecoverable. Closest reference is Mar 2-15, 2026 (~233/day).
- **One material change per campaign per window**, logged with a start date.
- **Never validate on a contaminated week.** Check `CONTAMINATION_LOG.md` before every comparison.
- **Day-of-week matched comparisons only.** Weekdays run 200-220, Saturdays 256-270. A raw
  seven-day mean across different weekend mixes is meaningless.
