# Quiet responders: what we'd build, seen from the person it happens to

DRAFT 2, 9 Oct 2026, for Helen. Still longer than one page; we tighten next.

**This brief is written as "what we would do if we're right."** It rests on a hypothesis that is not yet confirmed: that the 60-second ping wait in 4.2 made a few responders miss a run of pings, each miss lowered their ranking score by 0.12, and because the score only changes when someone is pinged, they never got the chance to climb back. Evidence: the code (`history.py`), a replay of the pings table, and the timing of Farlight's drop. Not yet checked: the live scores. If the four turn out to have normal scores, this brief changes.

Farlight is a cover name and nothing here tries to identify the person behind it. Sources: `00-rook/case-studies/farlight-timeline.md`, `farlight-next-steps.md`, `starved-responders-hypothesis.md`, `00-rook/code/dispatch-routing/`.

## 1. Who this is for

**Farlight, a responder in Uptown, and Linda Pruitt, the handler who speaks for her.**

Farlight is one of four responders (with The Undertow, Vesper and Meteor Mite) who went quiet after 4.2. Hers is the clearest case:

- Before 4.2 she got about 12 pings a week and took 73% of them.
- After 4.2 she got 11 pings in total over about 4 weeks, took 2, and missed 7 (64%). None in the week of 31 Aug.
- Uptown still had 6 to 10 callouts a week. Falkirk, Cindermark and Bulwark took her work, so no callout went unanswered. The cost landed on her and on them.
- Linda filed 11 tickets between 17 Aug and 5 Sep. All are open and none has a recorded reply. Farlight's own words, passed on: "starting to wonder if im still even in the system" (ticket 3120).

| Who | What they live with today |
|---|---|
| **Farlight** (responder, phone app) | Her phone goes quiet. She can't tell whether she was removed, penalised or unlucky, and nothing she does changes it. |
| **Linda** (her handler, web console) | Watches a responder go quiet, can't tell why, and has to guess what to say. Her only route is a ticket. |
| **Handlers like Kip** (secondary) | His Meteor Mite is starved while The Gale, in the same area, is swamped. He sees it and can't act on it. |

Why her and not an average: the aggregate acceptance rate hides this. Twelve other responders are fine, which is why it went unseen.

## 2. What changes for them once the fix exists

**The principle:** a bad few days no longer follows a responder indefinitely, and a quiet responder is visible to people instead of silent.

**The fix, in plain terms (decided for this draft):** a responder's recent-acceptance score eases back toward neutral (0.5) on its own as time passes, whether or not they are pinged. This answers the 2019 TODO in `history.py`: Wen's note weighed "a bad month shouldn't still be carrying it in the spring" against "someone who has stopped taking work should stay low until they take work again." We side with the first. Fading is slow enough that a real pattern of declining work still shows, but a stretch of misses can't become permanent.

**For Farlight**
- Her score starts climbing the day the fix ships, with no ping needed. A run of misses stops being a life sentence.
- As it rises she is reached again in her own area. She then earns her way up by taking pings, as everyone else does.
- Expected timing is weeks, not days (see the arithmetic in section 4). We should not promise her handler "within days."
- (To decide, Sofia) A line in the phone app that says she is in rotation and when she was last offered a callout. It answers her worry directly but goes beyond the routing fix.

**For Linda (and Kip)**
- A responder who has gone a set number of days without a ping shows on the console as quiet, with the date of their last ping. Linda no longer learns about it from the responder.
- For Kip: because the fade acts on everyone, a score pinned at the ceiling drifts down if the responder isn't pinged, so The Gale's lead over Meteor Mite narrows and load spreads. In Eastgate that is 21 vs 1 pings a week today.
- "Is she still on the list?" has a visible answer, not a ticket reply.

**For Support (Nadia)**
- The 30 routing tickets about starvation get a true reply and something to point at.

**What we'd measure** (draft, to agree with Ravi and Marcus): per-responder pings a week for the four (about 12 was the old level for Farlight), taken rate 70% or better, missed under 5%, checked two weeks and four weeks after shipping. Report per responder, not just in aggregate.

## 3. What the fix deliberately doesn't do

- **It does not revert 4.2.** Proximity weighting was a long-standing ask from wide-geography responders and stays.
- **It does not change the 60-second ping wait.** The wait and the weights shipped together and we can't yet separate them. The wait is a release config, not a handler setting. Revisit after Marcus replays 12 to 14 Aug one change at a time.
- **It does not change the proximity weight or the travel-time horizon.** See section 4: we think this is worth testing, but it isn't part of this fix.
- **It does not promise anyone a ping, a rank or a date.** It removes a trap; it isn't a quota. Someone who is unavailable or lacks the capability tags still isn't pinged.
- **It does not explain why these four miss about 4x as often as peers.** That is a separate open question (devices, push settings, shifts, how fast they usually answer). If they keep missing at 60 seconds, the fade only slows their slide. This is the biggest risk to the fix.
- **It does not add a handler override or a new runtime setting.** The existing override and audit log are unchanged.
- **It does not touch the Responder Availability Record.** Supply reads it to schedule maintenance and the shape of `current_record()` stays. Changing who gets pinged shifts callout load, so the Supply PM is told before shipping.
- **It does not try to identify anyone.** Everything works on cover names and routing scores.
- **It does not add mutual aid or shared cover.** Both are Q4 explorations, and Shared cover needs its own review against the confidentiality rule.
- **It is not a quiet reset of four numbers.** A one-off reset is the quickest relief but won't hold on its own. It could still be used as a bridge while the fade is built.

## 4. Revisit: do out-of-area and distance weighting keep starving the local responder?

Short answer: yes, partly, and the fade alone won't fully fix it. Worked from `routing.py` and `config.py`:

- **How the score works.** Proximity = 1 minus travel minutes ÷ 45, so each minute of travel is worth 0.0133 of score at the 4.2 weight (0.60). Recent acceptance, over its full range of 0 to 1, is worth only 0.25, about 19 minutes of travel. Before 4.2 it was worth 0.40 against 0.45, about 40 minutes. In effect, 4.2 halved how far a good or bad track record can offset distance.
- **What that means for Farlight.** At score 0 she trails a neighbour at 0.9 by about 0.22, equal to roughly 17 minutes of travel. After the fade has taken her back to 0.5 she still trails a 0.9 neighbour by 0.10, about 7.5 minutes. In a dense area like Uptown that gap may be small. If a neighbour is much closer, they win and that's arguably right. We can't tell which applies, because the repo has no travel times.
- **Fading both ways narrows this.** If the fade also pulls high scores toward 0.5, neighbours at 0.9 to 1.0 lose some of their edge over time, which also eases the load on The Gale and Falkirk. The cost is that the score says less about the responder, because everyone is pulled toward the middle.
- **The break-even is 60%.** A yes is +0.08 and a no or miss is -0.12, so a responder's score only drifts up if they take more than 60% of pings. At 50% they drift to 0. Peers sit around 66% taken (our estimate), so many are close to the line. The fade softens this but doesn't remove the unequal step sizes.
- **A miss and a turn-down cost the same.** A turn-down is a choice. A miss may be a phone, a shift or a missed notification. With a 60-second wait, misses are more likely to be circumstance. Worth considering a smaller penalty for misses.
- **Production differs from the code.** The code ranks only the callout's own area. Production pings neighbouring areas first about 1 in 5 times (before 4.2), and now far more often in Uptown, Harborside and Old Town. Our fix has to be tested against what production actually does, not the repo.
- **The fade rate is a real design choice.** With a half-life of 7 days (a placeholder), a score of 0 reaches about 0.13 after 3 days, 0.25 after a week and 0.375 after two. That fits "weeks, not days." A shorter half-life recovers faster but also forgives a responder who has really stopped taking work.

**Proposed order of changes (for Helen to react to):** (1) the fade, as above; (2) test the proximity vs. acceptance exchange rate and the miss penalty in the replay, and only change them if the replay shows the local responder still loses unfairly.

## 5. Still to check before this goes to Helen

1. **Live scores for the four** (near 0 supports the hypothesis; mid-range refutes it). Wen Li and Marcus.
2. **Do scores persist across releases and restarts?** In the repo they sit in memory. A reset at release would blunt the story.
3. **Travel-time gaps for Uptown, Harborside and Old Town callouts.** Needed to say how much the fade alone recovers.
4. **How the production candidate pool is built**, and why neighbours are first-pinged.
5. **Side effects on neighbours.** Falkirk is already at 7 Uptown pings a week and has an open grapple-line complaint.
6. **Quiet-responder flag.** Can Marcus compute "N days without a ping" from existing data, and what should N be?
7. **Fade rate and direction.** Half-life, and whether high scores fade as well. Replay August one setting at a time.

## Clickable version (Helen's second ask)

Not built yet. Suggestion: two screens, Farlight's phone view and Linda's console view, using the Farlight numbers above. Say if you want that next.
