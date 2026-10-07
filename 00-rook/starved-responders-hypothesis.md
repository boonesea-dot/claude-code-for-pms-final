# Why some responders are getting almost no pings — hypothesis for validation

Status: a hypothesis to test with Wen Li and Marcus, not a finding. Data: pings, callouts and responders tables in rook-database, 29 Jun to 6 Sep 2026 (before = to 11 Aug, after = from 12 Aug). Code: `00-rook/code/dispatch-routing/`.

## One sentence

Based on what I found, the reason some responders are getting almost no pings is that the 4.2 change to a 60-second wait made them miss a run of pings, and a miss lowers their ranking score with no way to earn it back, because the score only changes when someone is pinged, so once they rank below others nobody offers them callouts.

("No pings at all" is slightly strong: the four most affected still get about one ping a week.)

## What the data shows

- **Four responders collapsed.** Pings per week, before 4.2 to the last full weeks: Farlight about 12 to 1; The Undertow about 12 to 1; Vesper about 14 to 1–2; Meteor Mite about 11 to 1–2.
- **Misses came first.** In the week of 10 Aug all four had 4–5 missed pings out of about 10–12 (before that, 0–1 a week). Their pings fell in the following two weeks. This is the order the hypothesis predicts.
- **Their miss rate is far higher than everyone else's.** After 4.2: Farlight 64%, The Undertow 64%, Meteor Mite 57%, Vesper 53%. The other 12 responders are at 10–18%. The wait change alone does not explain why these four are so much worse.
- **Their own area's callouts are going to other people.** The share of callouts in the area whose first ping went to the area's own responder:

  | Area | Before 4.2 | After 4.2 |
  |---|---|---|
  | Uptown (Farlight) | 44 of 59 | 4 of 31 |
  | Harborside (The Undertow) | 50 of 63 | 8 of 32 |
  | Old Town (Vesper) | 61 of 72 | 5 of 38 |
  | Eastgate (Meteor Mite and The Gale) | 98 of 118 | 48 of 57 |
  | Westbury (Halfmoon) | 46 of 56 | 18 of 23 |
  | Northfield (Corporal Ashgrove) | 41 of 52 | 13 of 21 |

  Westbury and Northfield, which fell only about a third, barely changed.
- **The other end gains what the starved end loses.** In Eastgate, The Gale went from about 13 pings a week to 21 while Meteor Mite fell from 11 to 1. Eastgate is the one area with two responders.
- **Worth noting:** this means the code's region-only candidate list is not what production does. Callouts are being first-pinged to responders from other areas, so ranking across areas matters.

## Mechanism (from the code)

1. `config.py`: ping wait cut from 90s to 60s; proximity weight up (0.45 to 0.60); recent-acceptance weight down (0.40 to 0.25).
2. `history.py`: a miss costs the same as a turn-down (−0.12); a yes earns only +0.08; the score never fades back (the 2019 TODO).
3. The score only changes when a responder is offered a callout. A responder who has dropped gets fewer offers, so cannot recover.

## What the data does not tell us

- **Why these four missed so much more than others.** The tables have no time-to-answer for pre-4.2 pings. A guess is that they usually answered between 60 and 90 seconds, which the old wait allowed. This needs Wen Li or Marcus to confirm from raw logs.
- **Whether 4.2's weights or its shorter wait did more.** They shipped together.
- **Whether scores are stored between releases.** In the code they sit in a plain in-memory list. A reset at release would blunt the effect.
- **Why only about a third of unanswered local first pings get a second ping** (see `4-2-executive-summary.md`).
- **Whether "seasonal" played any part.** Prior-year August data from Ravi is still needed.

## Questions to validate (Wen Li, Marcus)

1. What are the current recent-acceptance scores for Farlight, The Undertow, Vesper and Meteor Mite, and for their peers? Near zero for the four and mid-range for the rest supports this; mid-range for the four refutes it.
2. Are scores stored between releases or restarts, and are they keyed by name?
3. When a callout is created, which responders are in the candidate list, and why are callouts in Uptown, Harborside and Old Town now first-pinged outside the area?
4. How long did these four usually take to answer before 4.2? Did many take 60–90 seconds?
5. Are the four marked available when callouts arrive?
6. Can August callouts be replayed with the old wait and old weights, one change at a time?

## What would prove it wrong

- Scores for the four are normal.
- Their pings fell without a prior rise in misses (the data above says they rose first).
- They were marked unavailable.
- Their areas simply had fewer callouts (they did: callouts per day fell about 15% overall, but these areas lost most of their first pings to other responders, which a fall in volume alone does not explain).
