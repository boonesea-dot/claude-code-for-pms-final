# Release 4.2: what we are observing in Dispatch

DRAFT for review. Prepared 7 Oct 2026 by the Dispatch PM. 4.2 shipped 12 Aug 2026.

## Context and caveats

Read these first; they limit what the observations below can support.

- **Sources and period.** rook-database (pings, callouts, responders, support tickets) for 29 Jun to 6 Sep 2026, the routing code in `00-rook/code/dispatch-routing/`, and the wiki's 4.2 page. Before is 29 Jun to 11 Aug (44 days); after is 12 Aug to 6 Sep (26 days). All comparisons are per day to allow for the unequal windows.
- **Two changes shipped together.** The ping wait was cut and the ranking weights changed in the same release. The data shows what changed after 12 Aug. It does not show which change caused which effect.
- **Correlation, not cause.** Everything here is timing and counts. Where we describe a mechanism, it is a hypothesis.
- **No prior-year data.** The database starts on 29 Jun 2026, so we cannot test the "August is always soft" explanation against earlier years.
- **Callout volume also fell.** Callouts per day dropped 15% from 12 Aug, for reasons we do not know. This affects how raw counts should be read.
- **Split date.** The window splits at midnight on 12 Aug, not the release morning, so a handful of pings are on the wrong side.
- **Code assumption.** We assume the routing code in the repo matches what is running.
- **Time-to-accept** is not in the pings table, so we cannot measure it.
- **Tickets and interviews.** Ticket themes are my grouping of subject lines, and many tickets are templated. The four handler interviews show which problems exist, not how common they are.

## What changed in 4.2

| Setting | Before | From 4.2 |
|---|---|---|
| Ping wait | 90s | 60s |
| Proximity weight in ranking | 0.45 | 0.60 |
| Recent-acceptance weight | 0.40 | 0.25 |
| Capability match weight | 0.15 | 0.15 |

The code treats a missed ping and a turned-down ping the same: both lower a responder's recent-acceptance score by 0.12. There is no mechanism for the score to recover on its own (open TODO from 2019).

## What we observe

### 1. More pings are going unanswered

| Share of pings | Before | After |
|---|---|---|
| Taken | 76.6% | 64.0% |
| Turned down | 21.1% | 18.0% |
| **Missed** | **2.3%** | **18.0%** |

- Turned-down pings take about 22 seconds to arrive in both periods, and their share is roughly steady. The change is in misses.
- The measured gap after a missed ping is 91-93s before 4.2 and 61-63s after, so the new ping wait is live as configured.
- The change is a step, not a slide. Misses were 0, 0 and 1 on 9-11 Aug, then 7, 12 and 8 on 12-14 Aug.
- The weekly acceptance rate was flat at 75-78% for six weeks, then 54% in the release week (a mixed week, since 4.2 shipped on the Wednesday).
- Weekly miss rate: 1-3% from 29 Jun to 3 Aug, then 21.5%, 17.7%, 14.8% and 12.7%. It is falling but still far above the earlier level. Chart: `00-rook/charts/missed-pings-by-week.png`.

### 2. Four responders have almost stopped receiving pings

| Pings per day | Before | After | Change |
|---|---|---|---|
| Farlight, The Undertow, Vesper, Meteor Mite | 7.0 | 2.2 | -69% |
| The other 12 responders | 17.7 | 21.3 | +21% |

- By late August the four were getting 0-4 pings a week, down from about 12.
- Their miss rate went from 2.9% to 59% of pings. For the other twelve it went from 2.1% to 13.9%.
- Callouts in the four responders' areas fell only 11-18%, in line with everywhere else. Their areas did not go quiet.
- The other twelve have absorbed the load, and their acceptance has largely recovered (about 74% by the last week).

### 3. More callouts are going unfilled

| Per day | Before | After | Change |
|---|---|---|---|
| Callouts | 20.0 | 16.9 | -15% |
| Pings sent | 24.7 | 23.5 | -5% |
| Pings taken | 18.9 | 15.0 | -20% |
| Pings per callout | 1.23 | 1.39 | +13% |
| Callouts where nobody took it | 5.5% | 11.1% | doubled |

- About three quarters of the 20% fall in pings taken follows the 15% fall in callouts, so raw counts overstate the effect of 4.2. The miss rate and the share of unfilled callouts are the better measures.
- 97 callouts were never taken (48 before, 49 after). Before 4.2, 45 of the 48 involved only turn-downs. After, 27 of 49 involve a miss.
- Southport went from 3 unfilled callouts to 9, and Riverside from 2 to 6. In 10 of those 15 after-4.2 cases, the area's one responder (Stormwrack, Ironvale) missed the only ping sent.
- Full list: `00-rook/data/unfilled-callouts.csv`.
- **A local responder's unanswered ping often ends the callout.** When the first ping goes to the responder in the callout's own area and isn't taken, only about a third of callouts get a second ping (22 of 64 before 4.2, 22 of 62 after). When the first ping goes to a responder from another area and isn't taken, a second ping always follows (172 of 172, then 121 of 121). This is the same before and after 4.2. After 4.2 there are more unanswered local pings (7.3% of callouts, then 14.1%), and about 65% of them end unfilled. This accounts for the 82 of 97 unfilled callouts that have only one ping. Fourteen of the 15 areas have a single responder, so it applies across the board. Southport and Riverside show it most clearly. We do not know whether it is a rule, a bug or a data artefact.

### 4. Handlers and responders are reporting the same thing

- Tickets rose from about 0.9 to 4.0 a day, starting in the release week; the first "few pings" ticket was dated about a week after the pings dropped. 45 of the 107 post-4.2 tickets are about routing.
- 15 tickets say a ping moved on before the responder could answer (nine handlers); 30 are about a responder getting few or no pings (four responders). None of either kind appear before 12 Aug, and all 30 of the second kind are still open.
- Tickets name Farlight, The Undertow, Halfmoon and Corporal Ashgrove. The pings data shows Farlight, The Undertow, Vesper and Meteor Mite as the most starved; Vesper and Meteor Mite have no tickets, and Halfmoon and Corporal Ashgrove are down only about a third. A ticket-only view would miss two of the four.
- 3 of 4 September handler interviews describe pings moving on too fast. Responders are asking whether they are still in the system.

## What we do not yet know

- How much of the effect comes from the shorter ping wait and how much from the routing re-weighting.
- Why callouts per day fell 15% from 12 Aug. Possible causes include seasonality and handlers working around Dispatch; the data cannot tell them apart.
- Why an unanswered ping to the area's own responder ends the callout about two thirds of the time, when the code walks down the whole ranked list.
- Why these four responders were hit hardest. A plausible mechanism is that misses lower a responder's rank and fewer pings follow, which fits the timing. But the acceptance weight went down in 4.2, which on its face should make misses matter less.
- Whether any of this is seasonal at the margin. A step on release day is a weak fit for a seasonal effect, but we cannot rule it out without prior-year data.

## Next steps

See the ranked task list in `00-rook/dispatch-priorities.md`. The first steps are replying to the starved responders' handlers, then the conversations with Marcus and Wen Li on the ranking logic, why an unanswered local ping ends the callout, and how to separate the ping wait from the weights.
