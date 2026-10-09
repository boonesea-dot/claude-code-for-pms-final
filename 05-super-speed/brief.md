# Quiet responders: what we'd build, seen from the person it happens to

For Helen · 9 Oct 2026 · Draft · **Owner: Sean Boone (PM, Rook Dispatch), for the brief and its outcome.** Our hypothesis: 4.2's 60-second wait caused a run of misses, each miss cut the responder's ranking score by 0.12, and because the score only changes when someone is pinged, they could never climb back. Two kinds of evidence fit this. *Modelled:* a replay of every ping through the code's rules puts all four at 0.00 by 6 Sep, and the other twelve at 0.72 to 1.00 (`00-rook/case-studies/farlight-next-steps.md`). *Owner-reported:* Sean reports the live scores for the four are near zero (9 Oct). There is no live-score export in the workspace, so this is a training-scenario input that Wen Li and Marcus still need to confirm. What is still unconfirmed is why these four started missing so much more than their peers. Farlight is a cover name; nothing here tries to identify anyone.

> ## The problem
> **Since 4.2, four responders have almost stopped getting pings, and nothing they do can change it** (their scores are modelled at 0.00 and reported near zero live, and only move when they are pinged). Their area's callouts still get answered, because other responders take them, so the aggregate numbers look fine. The responders are left wondering whether they've been dropped, and their handlers have no way to see or explain it.

> ## The fix
> **A responder's recent-acceptance score eases back toward neutral (0.5) on its own over time, pinged or not, and a responder who goes N days without a ping shows as quiet on the console (N starts at 7 days and is subject to change). An admin can also reset a responder's score to 0.5 once, so the four do not have to wait for the fade.** A bad stretch stops being permanent, and a quiet responder stops being invisible. This settles Wen's 2019 question in favour of "a bad month shouldn't still be carrying it in the spring."

## 1. Who it's for

**Farlight (Uptown), and Linda Pruitt, the handler who speaks for her.** One of four responders who went quiet after 4.2. Before: about 12 pings a week, 73% taken. After: 11 pings in about four weeks, 2 taken, 7 missed, none in the week of 31 Aug. Her area's callouts kept coming and neighbours took them, so nothing went unanswered, but she wonders "if im still even in the system" (ticket 3120). Linda filed 11 tickets; all are open with no recorded reply. Handlers like Kip see the other side: Meteor Mite starved, The Gale swamped (about 1 vs 21 pings a week). The aggregate hid this, because the other twelve responders are fine.

**The other three quiet responders** (Farlight's pattern repeats; rook-database pings table, before = 29 Jun to 11 Aug, after = 12 Aug to 6 Sep; weekly rates are the totals divided by 6.3 and 3.7 weeks):

| Responder | Pings a week, before | Pings a week, after | Taken, before | Taken, after | Missed, after |
|---|---|---|---|---|---|
| Farlight | 12 | 3 | 73% (55 of 75) | 18% (2 of 11) | 64% (7 of 11) |
| The Undertow | 12 | 4 | 77% (58 of 75) | 21% (3 of 14) | 64% (9 of 14) |
| Vesper | 14 | 5 | 81% (70 of 86) | 29% (5 of 17) | 53% (9 of 17) |
| Meteor Mite | 11 | 4 | 69% (48 of 70) | 21% (3 of 14) | 57% (8 of 14) |

Vesper and Meteor Mite filed no tickets (their handlers, Aunt Dot and Kip, were interviewed), so tickets alone understate this group. Samples after 4.2 are small (11 to 17 pings each), so read the rates as direction, not precision.

![Farlight's pings per week, and the illustrative score recovery with the fade](one-pager-visual.png)

## 2. What changes for them

- **Farlight:** her score starts rising the day it ships, with no ping needed, or jumps to 0.5 at once if an admin uses the one-time reset. She earns the rest by taking pings. Expect weeks, not days without the reset (a placeholder 7-day half-life takes a score of 0 to about 0.25 in a week and 0.375 in two).
- **The Undertow, Vesper and Meteor Mite:** the same fade applies from the day it ships, so their scores also start rising without a ping. They earn the rest by taking pings.
- **Linda and Kip:** the quiet flag shows the date of a responder's last ping, so "Is she still on the list?" gets a visible answer, not a ticket.
- **Measures** (at 2 and 4 weeks after ship):
  - **What the fix controls, per responder (all four):** the score. With the one-time reset it is 0.50 on day 1. Without it, the placeholder 7-day half-life gives about 0.375 from zero by week 2. Check the live score at 2 and 4 weeks.
  - **What we hope follows, if they stop missing:** pings a week back toward the before level above (11 to 14), 70%+ taken, under 5% missed. This is a target, not a promise: the reset and the fade both leave the cause of their misses open (section 3), and a 0.9 neighbour still leads by about 7.5 minutes of travel.
  - **Counts as not working:** the score has recovered but the four still miss more than 30% of pings or get under half their before-level pings at 4 weeks (30% and half are placeholders). Then the problem is the misses or the weights, not the stuck score, and the wait and weights come back into scope.
  - **Quiet flag:** no new "few or no pings" tickets are filed (30 are open today, naming Farlight, The Undertow, Halfmoon and Corporal Ashgrove; none before 12 Aug), and the flag lists the right responders when checked against the pings table.

## 3. What it deliberately doesn't do

- **Revert 4.2.** Proximity weighting was a long-standing ask and stays.
- **Change the 60s wait, or the proximity weight.** We can't separate the wait from the weights yet. The fade restores her only to neutral: a neighbour on 0.9 still leads by about 7.5 minutes of travel, and 4.2 halved how far a good record can offset distance. Test both in a replay before touching them.
- **Promise anyone a ping, rank or date.** It removes a trap; it is not a quota.
- **Explain why these four miss about 4x as often as peers.** Biggest risk: if they keep missing at 60s, the fade only slows the slide.
- **Add a handler override or setting** (the reset is admin-only, once per responder, and not a handler control), touch the availability record's shape, or use anything but cover names.
- **Add mutual aid or shared cover** (Q4 explorations; shared cover needs a confidentiality review).
- **Settle Supply.** Supply schedules gear servicing into "quiet" windows from the availability record. Starvation may have made the four look idle, and neighbours are carrying extra load. The fix shifts load again. *Open question now:* how does Supply define a quiet window, and has it booked servicing for the four since 12 Aug? (The tell-before-ship and follow-up steps are under "Also happens".)

## Also happens

Effects and follow-ups that come with the fade and the flag but are not the fix itself:

- **Swamped responders' lead narrows.** The fade works on everyone, so a responder with a high score (like The Gale) drifts back toward 0.5 too, and their share of pings may fall.
- **Support (Nadia):** the 30 open starvation tickets get a true reply once we know the cause is confirmed.
- **Supply:** tell the Supply PM before ship. Fast follow: check Supply's scheduling for the four at 2 to 4 weeks (4 post-4.2 tickets are the baseline).
- **N is a starting value.** The quiet flag shows a responder with no ping in N days. N starts at 7 and is subject to change; the half-life (7 days, placeholder) and N are for Marcus to confirm.

## Before this goes further

Wen Li and Marcus to confirm: live scores for the four (Sean reports near 0; ask them to confirm and give the source), whether scores persist across releases, how production builds its candidate pool (it pings neighbouring areas first, unlike the repo), and travel-time gaps in Uptown. Marcus to replay 12 to 14 Aug one change at a time and set the fade rate. A clickable prototype (Farlight's phone view and Linda's console) is in `05-super-speed/prototype.html`, including the admin one-time reset to 0.50.

## Sources

- Section 1 table: queried from the pings table (rook-database) on 9 Oct 2026, before = 29 Jun to 11 Aug, after = 12 Aug to 6 Sep.
- 59% vs 14% missed after 4.2, four vs the other twelve (about 4x): `00-rook/case-studies/farlight-next-steps.md`.
- Score rules, replay and relief options: `00-rook/case-studies/farlight-next-steps.md`, `00-rook/starved-responders-hypothesis.md`.
- Weighting ("halved" the offset; the 7.5-minute gap): calculated from the weights in `00-rook/code/dispatch-routing/config.py` (proximity 0.60, was 0.45; 45-minute horizon). The working is in `05-super-speed/one-pager-draft-2.md`, a superseded draft.
- Meteor Mite vs The Gale (about 11 to 1 vs 13 to 21 pings a week): `00-rook/starved-responders-hypothesis.md`.
- "Other twelve are fine": the replay and miss rate above, not a separate check.
