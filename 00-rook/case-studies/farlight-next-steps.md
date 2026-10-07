# Farlight: next steps to restore ranking and pinging (internal)

DRAFT, 7 Oct 2026. Applies to the four starved responders (Farlight, The Undertow, Vesper, Meteor Mite), with Farlight as the worked example.

## What the code and data now suggest

Source: `00-rook/code/dispatch-routing/` (`history.py`, `routing.py`, `offer.py`, `config.py`) and a replay of the pings table.

- Every ping that isn't taken, whether turned down or missed, costs a responder 0.12 of their recent-acceptance score. Each taken ping adds 0.08. The score runs from 0 to 1 and never recovers on its own (the 2019 TODO in `history.py`).
- A responder only gains score by taking a ping. Ranking never removes anyone, but a responder at the bottom of the list is only reached when everyone above has failed to answer.
- I replayed every ping in the table through those rules, starting everyone at 0.5 on 29 Jun. Before 4.2 the four scored 0.84 to 1.00. By 6 Sep all four were at 0.00. The other twelve ended between 0.72 and 1.00, and the lowest dip for any of them was 0.28.
- At the 4.2 weights, recent acceptance can move a responder's score by at most 0.25, against 0.60 for proximity. A gap of about 0.2 in acceptance is worth about 15 minutes of travel time. That is enough to push a responder below their neighbours in a dense area such as Uptown, where Falkirk, Cindermark and Bulwark picked up her work.
- Assumptions: the repo code matches production; scores start at 0.5; nothing resets them (for example a service restart; `_scores` is held in memory in the repo version). This is a replay, not live data. It shows the rules fit the pattern, not that they caused it.

Open question that matters for the fix: why did these four miss about 59% of pings after 4.2, against 14% for the other twelve? If we lift their rank without understanding that, they may miss again and fall back.

## Steps, in order

1. **Confirm the mechanism (Wen Li, Marcus).**
   - Ask for the live recent-acceptance score of the four. If they are at or near 0, the mechanism is confirmed.
   - Ask whether scores reset on restart or decay anywhere.
   - Ask where ties and the position in the list are decided for Uptown.
2. **Find out why these four miss so much (Marcus, Sofia, Nadia).** Compare their devices, push settings, shift patterns and areas against the other twelve. 4.1 changed push reliability, and three of four interviewees wanted alerts easier to notice. Ask their handlers (Linda, Desmond, Aunt Dot, Kip).
3. **Decide the relief option (Marcus, Wen Li, then Helen).** Rank them by speed and risk:
   - *One-time reset of the four scores* to a neutral or pre-4.2 value. Fastest, a data change, not a release. Needs Marcus to confirm it is possible and safe. On its own it will not hold: with the 60s wait, they may start missing again.
   - *Reset plus a longer ping wait for these four, or for everyone,* shipped as a 4.2.x point release. Slower, because config ships with releases.
   - *Make a miss cost less than a turn-down, or let scores ease back toward 0.5 over time.* The fix to the cause, not just the four. This is the 2019 TODO. It affects everyone, so it needs a test first.
   - *A rank floor for responders with a long gap since their last ping.* Keeps anyone from being starved while the rest is decided.
4. **Separate the ping wait from the weights (Marcus, Wen Li).** Replay 12 to 14 Aug with the old 90s wait and old weights, one change at a time (task 7). Without that we can't say which of the two 4.2 changes to adjust.
5. **Get a decision (Helen).** Frame it as a targeted correction, not a revert of 4.2. Priya's concern was that proximity weighting was a long-standing ask, and nothing here changes that.
6. **Check side effects before shipping.**
   - Raising the four lowers pings for neighbours such as Falkirk (already at 7 Uptown pings a week and reporting grapple-line problems). Check that nobody else is starved.
   - Callouts per day fell 15% from 12 Aug for an unknown reason.
   - Tell the Supply PM. Dispatch writes the availability record Supply reads, and a change in who gets pinged changes callout load per responder. Do not change the shape of `current_record()`.
7. **Define success and who watches it.**
   - Historic level for Farlight: about 12 pings a week, 70%+ taken, under 5% missed. Have Ravi report weekly per responder for the four, not just in aggregate.
   - Decide a check date, for example two weeks after any change.
8. **Close the loop with the handlers.** Send the draft in `farlight-handler-reply-draft.md` now, then update Linda when step 1 or 3 gives an answer.

## What not to do

- Don't reset the scores quietly and call it fixed. The cause of the high miss rate and the ping wait are still open.
- Don't promise the four a date or a rank to their handlers until step 5.
- Don't try to work out who anyone is. Everything here is about cover names and routing scores.

## Links to the priorities list

Tasks 1 (replies), 3 and 5 (Wen Li), 7 (separate ping wait from weights), 8 (interim relief), 9 (decision date). New: step 2 (why these four miss) is not on the list yet.
