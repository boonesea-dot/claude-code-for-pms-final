# Rook Dispatch: priorities and open questions

Living file. Update the status boxes and the log as work happens. Created 2026-10-06 from the handover doc and the rook-wiki Company section. Evidence added the same day from the support tickets and the four customer interviews. The pings, callouts, handlers and responders tables, the routing code and most of the wiki have not been read yet.

Status key: `[ ]` open, `[~]` in progress, `[x]` done. Items marked **NEW** were added when the list was restructured on 2026-10-06.

## Reprioritization rule (always follow)

Tasks are one list, ranked by the impact of resolving each. Judge impact on these four things together; never rank on ticket or interview counts alone.

- **Severity for the persona affected.** Consider every persona: responders (the end users on their phones), handlers (the console), support (Nadia's team), product (me), and Supply users such as quartermasters. A problem that is rare but harms safety, income, access or trust can outrank a common annoyance. Examples: a cracked vest plate waiting 11 days, a screen reader user who cannot use the console, a responder who thinks they have been dropped from the system.
- **Breadth.** How many people it reaches, as one input only. Counts are skewed by who files tickets (a few handlers filed most of the "quiet responder" tickets) and by who was interviewed, so check how many distinct people and personas are behind a number.
- **Unblocking.** Items whose results change or enable other tasks rank above the tasks they unblock (for example, reading the pings table comes before deciding on interim relief).
- **Cost and urgency.** A cheap action that stops ongoing harm (such as a holding reply to a waiting handler) can rank above a larger task with a similar impact.

When ranking, say which of these drove each move. Whenever this file is updated in any way (status change, new task, new evidence, a task dropped), stop and consider whether the order should change.

- Do not reorder the list on your own. Show the user a table in chat with the current order and the proposed order side by side, and flag any new tasks clearly.
- Apply the new order only after the user agrees. Status, evidence and log updates can be made without that step.
- When the order changes, note it in the Log with the date and what moved.

## Tasks, highest impact first

1. [ ] **Find out whether acceptance is down, and why.** Priya's "seasonal, back in September" is untested.
   - Ask Ravi Menon for weekly acceptance rate and time-to-accept from before 4.2 (12 Aug) to now, with turned-down and missed split out and a by-geography cut. Also prior-year Aug/Sep for the seasonal check.
   - Check whether rook-database has this directly. Tasks 2-5 are the first steps.
   - Decide: routing change, 60s ping wait, or season? Can the two be separated by date or segment?
   - *Evidence so far (see Evidence):* two separate patterns, pings vanishing before a responder can answer and some responders getting few or no pings. Both started after 4.2, so "seasonal" looks unlikely. Check acceptance by responder, starting with Farlight, The Undertow, Halfmoon, Corporal Ashgrove (ticket-heavy) and Meteor Mite vs The Gale (Kip's interview).
2. [ ] **NEW: Test the "seasonal" claim with callout volume.** Count callouts per week from 29 Jun to 6 Sep in the callouts table. If volume did not dip in August, "soft August" is not the explanation. The table has no prior year, so ask Ravi for that as well.
3. [ ] **Check the pings table for the starved responders.** Pings per week before and after 12 Aug for Farlight, The Undertow, Halfmoon, Corporal Ashgrove, Meteor Mite and The Gale. Were they ranked low, or just not reached? Also look for other responders with the same drop.
4. [ ] **Measure the real ping wait in the pings table.** Confirm it is 60s, whether it varies, and how long pings actually last. Ticket 3043 asks someone to confirm the timing.
5. [ ] **NEW: Read the routing code and CHANGELOG** in `00-rook/code/dispatch-routing/` (README, CHANGELOG, `routing.py`, `config.py`, `history.py`, `offer.py`, `availability.py`). It may show what 4.2 changed and how ranking works, which would answer part of task 17.
6. [ ] **Meet Wen Li on how ranking works.** She is the only source. Get the routing score inputs, old vs new weights, and what a miss costs a responder's rank.
7. [ ] **Ask Wen Li whether a responder who falls in rank can recover.** If missed pings lower recent acceptance and lower rank, a responder starved of pings may never climb back. Check against the Meteor Mite / The Gale split.
8. [ ] **Answer the 30 open tickets about starved responders.** Handlers are being told nothing, and responders are asking whether they are still in the system (Farlight, The Undertow). This is a retention risk separate from the fix.
9. [ ] **Define the 4.2 problem and a decision date.** State the question and what we'd do under each answer. Options short of a full revert, e.g. reverting only the ping wait or varying it by responder group.
10. [ ] **NEW: Interim relief options for starved responders.** While the cause is investigated, what could help them now (for example a rank floor, or a longer ping wait for a period)? Weigh against Priya's warning not to revert 4.2 and decide with Marcus and Helen.
11. [ ] **Check with Marcus Oyelaran** on the current state of 4.2, available telemetry, anything shipped in August not in the notes, and whether ping wait can be tested separately from routing weights. Reason it matters: 15 post-4.2 tickets and 3 of 4 interviewees describe pings moving on too fast.
12. [ ] **Talk to Helen Achebe about commitments.** Not yet had, per Priya.
    - Which Q3 items are still commitments (Availability Confidence is Committed for 4.2 but not in the release)?
    - Was Ping timeout tuning "tuned", or just cut from 90s to 60s?
    - Who owns Requisition approval chains (4.3)? What is the release cadence now?
    - What does she want from me in the first 30 days?
13. [ ] **Fix the acceptance metric.** Split turned-down from missed; report by geography and by responder. "First ping in 5 days and she missed it" tickets show why the split matters.
14. [~] **Standing 15 minutes with Nadia Hoffmann.** Ticket analysis done from the database (see Evidence). Take it to her. 30 routing tickets about starved responders are all still open, up to about 3 weeks old. Ask who owns them.
15. [~] **Read the September interviews and talk to Sofia Marino.** Interviews read 6 Oct; still to do: talk to Sofia. Ask her about the leading question on quiet weeks in the Aunt Dot interview and what else the full recordings contain.
16. [ ] **Refresh the roadmap.** Last reviewed 30 Jun, all items owned by Priya, Q3 is over. Re-own, mark shipped/slipped, set what's next.
17. [ ] **Write the ranking description** (Priya's ask) from tasks 5 and 6; put it in the wiki next to the Glossary.
18. [ ] **Supply feedback to pass on (not Dispatch):** the requisition queue treats a cracked vest plate like boot laces (Halloran, 11 days), field failure reports get no reply, catalog search is weak. Pass to the Supply PM. This may support "Requisition approval chains" (4.3).
19. [ ] **Ask the Supply PM whether they saw changes to maintenance scheduling after 4.2** (Dispatch writes the Responder Availability Record; Supply reads it). Evidence is mixed: 4 post-4.2 tickets about bad booking dates and late reminders, but Halloran said scheduling has improved and avoids busy weeks.
20. [ ] **Find and read Security Policy 4.1** (not in the wiki). Do this before looking at Shared cover between responders.
21. [ ] **Read the Routing Override Audit Log page;** a rise in handler overrides after 4.2 would be a signal.
22. [ ] **Look at smaller console asks:** an undo or short delay on accidental turn-downs (ticket 3033), distinct alert sound per responder, a second notification for the handler, larger status text, dark mode (Kip's top ask; tickets 3018, 3029, 3105, 3117), a warning when filters reset.
23. [ ] **NEW: Tag support tickets by theme.** Themes in this file were guessed from subject lines. A tag field would make the next count reliable.
24. [ ] **Pick one term, "ping wait" or "ping timeout",** and use it everywhere.
25. [ ] **Q4 exploration items** (Handler phone app, Shared cover, mutual aid) only after the 4.2 question is settled. The handler phone app is relevant: Aunt Dot asked to be told when a callout arrives, not just the responder.

## Evidence (tickets and interviews)

Ticket counts (147 tickets, 29 Jun - 7 Sep, from rook-database; themes assigned by me from subject lines, 12 Aug counted as post-4.2; pre window 44 days, post 27):

| Theme | Pre | Post | Open |
|---|---|---|---|
| Ping moved on before responder answered | 0 | 15 | 15 |
| A responder getting few or no pings | 0 | 30 | 30 |
| Console filters | 2 | 14 | 6 |
| Notifications and alert sounds | 5 | 1 | 1 |
| Supply maintenance scheduling | 0 | 4 | 2 |
| Supply gear, requisitions, quality | 10 | 12 | 7 |
| Console UX, accounts, other | 23 | 31 | 22 |
| **Total** | **40** | **107** | |

- Tickets per day rose from about 0.9 to 4.0. 45 of the 107 post-4.2 tickets are about routing.
- The ping-moved-on tickets come from nine handlers about nine responders, so they are widespread.
- The few-pings tickets are about only four responders: Farlight (11), The Undertow (11), Halfmoon (5), Corporal Ashgrove (5). The first is dated 17 Aug and filing continued to 7 Sep.

### Ticket quotes by theme

Handlers wrote these, sometimes passing on a responder's own words. Counts are tickets in that theme.

**Ping moved on before the responder could answer (15 tickets, 9 handlers)**
- "phone buzzed, by the time i unlocked it, gone." (3048, passing on Sgt. Falkirk's words)
- "Captain Vantage was in the process of suiting up, coat and one boot on, when it moved along to someone else before he could get to his phone... I would ask that whoever reviews this at least confirm the timing is what it is meant to be, since it felt very fast." (3043, filed 13 Aug)
- "Is there a set amount of time before it moves on?" (3047)
- "She says it has happened more than once lately." (3052)

**A responder getting few or no pings (30 tickets, 4 responders)**
- "nothing again this week. starting to wonder if im still even in the system." (3120, 3121; Farlight and The Undertow's own words)
- "Farlight has had four pings in the past seven days. Before the middle of August she was getting about twelve a week." (3071)
- "She has started asking me every day whether something is broken. I do not know what to tell her." (3103)
- "the first in five days, and it was gone before she could answer. Given how long she had been waiting, this landed badly." (3110; 3140 says the same for The Undertow after nine days)
- "I would like to understand whether something has changed with his account. I would rather know than guess." (3104)
- The drops differ in size. Farlight about 12 to 2-4 a week and The Undertow about 12 to 1-6 are severe. Corporal Ashgrove about 10 to 6-7 and Halfmoon about 11 to 7-8 are mild, about a third.

**Console filters (14 tickets, 2 of them thanks)**
- "Having the console keep my filters has saved me a few minutes every shift." (3062)
- "I did not notice for a while and was looking at the wrong list. Could it warn me when that happens?" (3064)
- "The console kept my filters, but it kept the ones from my colleague's shift, not mine. We share a computer." (3046)

**Notifications and alerts (6 tickets, 5 before 4.2)**
- "Every alert on the console makes the same sound, so I have to look up every time to see what it is." (3031)
- "The second one showed up after she had already said yes." (3016)

**Supply maintenance scheduling (4 tickets)**
- "booked Sgt. Falkirk's suit in for servicing on the day of the city marathon, which is always one of our busiest days." (3054)
- "arrived two days after the service date. It would be more useful a week before." (3057)

**Supply gear, requisitions, quality (22 tickets)**
- "Farlight has a cracked vest plate... the priority field on the requisition does not seem to change how quickly it moves. How do I mark something as genuinely urgent?" (3021; 3074 repeats it for Nightwell)
- "Sgt. Falkirk's grapple line reels back in much slower when it is below freezing... I have filed a field failure report as well, but wanted to raise it here in case it is a known issue." (3022; 3092 repeats it for Corporal Ashgrove)
- "I can see it on the item's history, but nobody has been in touch. Does anyone read these?" (3032)
- "nine days ago and it is still waiting for a quartermaster to sign it." (3063)

**Console UX, accounts, other (54 tickets)**
- "I work nights and it is the brightest thing in the room by a long way." (3018, dark mode; also 3029, 3105, 3117)
- "Several buttons on the console are read out as 'button' with no name, so she cannot tell them apart." (3039, screen reader; 3137 repeats it)
- "I keep notes about medical and equipment needs there and it is not enough room." (3003, notes cut off at about 250 characters; 3090 repeats it)
- "Our invoice this month lists Halfmoon twice. She is one person." (3023; 3075 says the same for The Undertow)
- "Can she have her own sign-in for Ironvale's account rather than using mine?" (3053; 3141 repeats it)
- "I am watching it, not clicking it, and then I have to sign in again at the worst moment." (3020, logged out too often)

Notes: ticket 3043 says "yesterday evening" (about 12 Aug, release day) while Mr. Ambrose's interview puts a similar boot-half-on moment about two weeks before 2 Sep. It may be the same incident described loosely, or two. Many tickets are templated (identical wording with only the name changed), so the counts show how often the same pattern recurs rather than 122 distinct stories.

Interviews (4 handlers, 2-5 Sep): ping moves on too fast 3 of 4 (Aunt Dot, Mr. Ambrose, Halloran); alerts missed or indistinguishable 3 of 4; quiet vs swamped responders 2 of 4 (Aunt Dot, Kip); text too small 2 of 4; dark mode, filter resets, Supply complaints and tag legend 1 each. Quote worth remembering, Aunt Dot: "by the time he's actually got a thumb on the screen, it's gone." Kip: two cards on the same screen "might as well be two different products."
Caveats: four interviews show which problems exist, not how common they are. The quiet-weeks answer from Aunt Dot was prompted by the designer.

## Contradictions and gaps in the docs

- **Cadence:** docs say monthly releases; actual dates are 4.0 (7 Apr), 4.1 (16 Jun), 4.2 (12 Aug). Nothing for 4.3, though Requisition approval chains is committed to it.
- **Ping wait:** a committed roadmap item ("Ping timeout tuning") but described in the handover as incidental. Also two names for the same thing.
- **Squeezed items:** Priya says "a couple"; the roadmap shows only Availability Confidence still unshipped.
- **No numbers anywhere in the docs:** no acceptance, time-to-accept, miss rate or ticket counts, before or after 4.2. (The database does have tickets, callouts and pings; tickets now analysed, pings not yet.)
- **"Seasonal" claim contradicted by evidence:** Priya's view does not fit the tickets (nothing like them before 4.2, concentrated in four named responders) or the interviews. Still not proven until the callouts and pings tables are checked.
- **Maintenance:** 4 tickets say scheduling is worse after 4.2; Halloran says it has improved.
- **Quiet responders beyond the ticket four:** Kip's Meteor Mite is quiet while The Gale is swamped. Neither is in the ticket group.
- **Metric design:** acceptance rate lumps turned-down with missed, and is reported only in aggregate.
- **Ranking:** no documented weights or before/after values for the 4.2 proximity change.
- **Roadmap:** stale (30 Jun) and all items owned by a departed PM. Requisition approval chains is a Supply item with no known Supply owner.
- **Mutual aid:** Glossary says it's on the Q4 exploration list; the roadmap database doesn't list it.
- **Security Policy 4.1** is referenced in About Rook but isn't in the wiki.
- **Wen Li** was away 14-24 Aug, covering the 4.2 release and the first complaints. Who reviewed the routing change?

## Pages not yet read

Research (the rest of it, including the Change to who gets pinged row), Product briefs, Handler Phone App, Bulk Callout, Requisition Approval Chains, Routing Override Audit Log, page comments (except two roadmap rows), `00-rook/code/dispatch-routing/` (README, CHANGELOG, code), and the callouts, pings, handlers and responders tables in rook-database.

Read since the first version: the four Customer interviews and the support_tickets table.

## Log

- 2026-10-06: File created. No items started.
- 2026-10-06: Analysed 147 support tickets and read the four customer interviews. Added 6 tasks and an Evidence section; started two tasks; updated the contradictions list.
- 2026-10-06: Added ticket quotes by theme to Evidence, and widened the reprioritization rule to weigh severity per persona, breadth, unblocking and cost, not occurrence counts. Order not yet changed; a proposal was shown in chat.
- 2026-10-06: Restructured from weekly groups into one list ranked by impact, and added the reprioritization rule. Old-to-new numbers: 1→1, 2→6, 3→12, 4→15, 5→14, 6→11, 7→9, 8→13, 9→17, 10→16, 11→19, 12→20, 13→24, 14→21, 15→25, 16→3, 17→4, 18→8, 19→7, 20→22, 21→18. New tasks: 2, 5, 10, 23.
