# Quiet responders: what we'd build, seen from the person it happens to

DRAFT 1, 9 Oct 2026, for Helen. Longer than one page on purpose; we tighten next. Farlight is a cover name and nothing here tries to identify the person behind it. Sources: `00-rook/case-studies/farlight-timeline.md`, `farlight-next-steps.md`, `starved-responders-hypothesis.md`, `00-rook/code/dispatch-routing/history.py`.

## 1. Who this is for

**Farlight, a responder in Uptown, and Linda Pruitt, the handler who speaks for her.**

Farlight is one of four responders (with The Undertow, Vesper and Meteor Mite) who went quiet after 4.2. Hers is the clearest case:

- Before 4.2 she got about 12 pings a week and took 73% of them.
- After 4.2 she got 11 pings in total over about 4 weeks, took 2, and missed 7 (64%). She got none in the week of 31 Aug.
- Uptown still had 6 to 10 callouts a week. Others (Falkirk, Cindermark, Bulwark) took her work, so no callout went unanswered. The cost landed on her and on them.
- Linda filed 11 tickets between 17 Aug and 5 Sep. All 11 are open and none has a recorded reply. Farlight's own words, passed on: "starting to wonder if im still even in the system" (ticket 3120).

Two people feel this, and the brief should serve both:

| Who | What they live with today |
|---|---|
| **Farlight** (responder, phone app) | Her phone goes quiet. She can't tell whether she was removed, penalised or unlucky, and nothing she does changes it. |
| **Linda** (her handler, web console) | Sees a responder go quiet, can't tell why, and has to guess what to say. Her only route is a ticket. |
| **Handlers like Kip** (secondary) | One of his responders, Meteor Mite, is starved while The Gale, in the same area, is swamped. He sees it and can't act on it. |

Why her and not an average: the aggregate acceptance rate hides this. Twelve other responders are fine, which is exactly why it went unseen.

Caveat on cause: the mechanism is a hypothesis built from the code and a replay, not yet confirmed against live scores (see `starved-responders-hypothesis.md`). The brief should still hold if the cause turns out to be something else, because it describes what the person experiences.

## 2. What changes for them once the fix exists

**The principle:** a bad few days no longer follows a responder for months, and a quiet responder is visible to people instead of silent.

Today, in the code, a miss costs 0.12 of a responder's score and a yes adds back only 0.08. The score changes only when they are pinged, so a responder who drops is pinged less and can't climb back (the 2019 TODO in `history.py`). The fix is a way back.

**For Farlight**
- She is offered callouts in her own area again, within days, not weeks. A run of misses stops being permanent.
- Taking pings again moves her up, and the app gives her no reason to wonder whether she is still in the system.
- (To decide) A line in the phone app that says she is in rotation and when she was last offered a callout. This is what would answer her worry directly, but it is a design call for Sofia and goes beyond the routing fix.

**For Linda (and Kip)**
- They stop learning about this from the responder. A responder who has gone a set number of days without a ping shows up on the console as quiet, with the date of their last ping.
- For Kip: Meteor Mite and The Gale in Eastgate share the load again, instead of 1 and 21 pings a week.
- When Linda asks "is she still on the list?", the answer is visible, not a ticket reply.

**For Support (Nadia)**
- The 30 routing tickets about starvation stop being unanswerable. There is something to point at and a reply that is true.

**What we'd measure** (draft, to agree with Ravi and Marcus): per-responder pings a week for the four (about 12 is the old level), taken rate 70% or better, missed under 5%, checked two weeks after any change. Report per responder, not just in aggregate.

## 3. What the fix deliberately doesn't do

- **It does not revert 4.2.** Proximity weighting was a long-standing ask from wide-geography responders and stays.
- **It does not change the 60-second ping wait.** We can't yet separate the wait from the weights (they shipped together), and the wait is a release config, not a handler setting. Revisit once Marcus can replay 12 to 14 Aug one change at a time.
- **It does not promise anyone a ping, a rank or a date.** It removes a trap. It doesn't guarantee Farlight work, and nobody gets a quota. If someone is unavailable or lacks the capability tags, they still aren't pinged.
- **It does not explain why these four miss about 4x as often as their peers.** That is a separate open question (devices, push settings, shifts, how fast they usually answer). If we lift their rank without answering it, they may miss again and fall back.
- **It does not let handlers override routing from this feature.** The existing override and audit log are unchanged, and no new runtime setting is added.
- **It does not touch the Responder Availability Record.** Supply reads it to schedule maintenance. The shape of `current_record()` stays as is. Changing who gets pinged shifts callout load, so the Supply PM is told before shipping.
- **It does not try to identify anyone.** Everything works on cover names and routing scores. There is no mapping to legal identity and the fix assumes none.
- **It does not add mutual aid or shared cover.** Those are Q4 explorations and Shared cover needs its own review against the confidentiality rule.
- **It is not a quiet reset of four numbers.** A one-off reset of the four scores is the quickest relief, but on its own it won't hold. Helen's point stands: the number is the easy part, and the fix is the behaviour above.

## Still to check before this goes to Helen

1. **Live scores.** Are the four near 0 (supports the mechanism) or mid-range (refutes it)? Ask Wen Li and Marcus.
2. **Do scores persist across releases and restarts?** In the repo they sit in memory.
3. **Production vs code.** The code ranks only the callout's own area; production pings neighbouring areas first. The fix has to apply to what production does.
4. **Side effects on neighbours.** Falkirk is already at 7 Uptown pings a week and has an open grapple-line complaint.
5. **Quiet-responder flag.** Is "N days without a ping" something Marcus can compute from data we already have? What N?
6. **Wen Li's intent.** Her 2019 note argued both sides: a bad month shouldn't last into spring, but someone who has stopped taking work may be better left low. The brief should say which we're choosing and why.

## Clickable version (Helen's second ask)

Not built yet. Suggestion: two screens, Farlight's phone view and Linda's console view, using the Farlight numbers above. Say if you want that next.
