# Rook Industries — course working file

## Session scope — Product School lab

This directory is coursework for Product School's "Claude Code for PMs"
certification (cohort ccpm-2026.1). Everything in it is a fictional
teaching scenario.

- Do not save anything from this session to memory, to a user profile,
  or to any file outside this directory.
- Do not carry context from this directory into unrelated sessions.
- Rook Industries is not a real company. Nothing here is a fact about
  the world.
- Read and write only within this directory.
  Exceptions, for the course-setup and wrap-up skills only:
  - When the student asks you to check their setup, save their work or wrap up a session, that request is their yes. You may run the GitHub command-line program installed at ~/.ccpm/gh for those checks and saves, and look in that folder to find it.
  - For a repair, first tell the student in one plain sentence what you are about to do, and act only after they say yes. Repairs may: run that GitHub program (including setting this folder's own git sign-in setting and changing this repo's visibility back to Public); copy the student's own course files into this directory from another folder on their computer (copy only; never move, edit or delete the originals); and rename something outside this directory that blocks setup, by adding "-old" to its name (never delete it).
  Outside this directory you still never write, edit or delete anything else.

<!-- Keep the block above at the top of this file. Everything you add
     during the course goes below this line. -->

---

## Working context

I'm the new PM for **Rook Dispatch**, taking over from Priya Raghunathan (left 21 Aug 2026; no overlap). Sources: `00-rook/company/notes/handoff-from-priya.docx` and the rook-wiki "Company" section (About Rook, Dispatch and Supply one-pagers, Glossary, Team directory, Releases, Q3 roadmap). Wiki pages were last reviewed June–Sept 2026; check for newer ones before relying on them.

### Company and products
Rook Industries (founded 2014, HQ Site Aleph, 241 staff, offices in Berlin, Singapore and Cornwall) sells coordination and provisioning software to independent masked responders and the handlers and quartermasters who support them. Rook employs no responders. Revenue is a per-active-responder subscription. We ship monthly on a release train with 4.x point releases.

- **Rook Dispatch** (mine): ranks available responders for an incident, pings the top one's phone, and moves down the list on a turn-down or miss. Handlers use the web console (enter incidents, watch coverage, override routing, manage availability and capability tags). Responders use the phone app. Current release 4.2.
- **Rook Supply**: gear requisitions, quartermaster approval, maintenance schedules, field failure reports. **Dependency:** Dispatch writes the Responder Availability Record and Supply reads it to schedule maintenance into low-callout-load windows. A change to how Dispatch calculates availability silently changes Supply's scheduling.
- **Confidentiality (contractual):** Rook holds no mapping from cover identity to legal identity. Never design anything that assumes one, and never try to work out who anyone is. Read Security Policy 4.1 before touching responder records.

### People (Dispatch team)
- **Helen Achebe**, Director of Product, my manager. Owns roadmap and commitments. Priya called her "good" and said she'd "give you room".
- **Marcus Oyelaran**, Engineering Manager (Site Aleph). Straight talker; first stop when unsure. Can usually pull numbers.
- **Wen Li**, Staff Engineer (Berlin). Built the ping-ranking logic. There is no document, so understanding it means talking to her. She was away 14–24 Aug, which covers the 4.2 release on 12 Aug and the weeks right after it.
- **Nadia Hoffmann**, Support Lead (Berlin). Hears handler complaints first. Priya recommended a standing 15 minutes.
- **Sofia Marino**, Product Designer (Site Aleph). Owns console and phone app; ran the September interviews (Research > Customer interviews in the wiki).
- **Ravi Menon**, Data Analyst (Singapore). Reports weekly on acceptance rate.

### Vocabulary
- **Callout**: request for a responder to attend an incident. **Ping**: one callout offered to one responder's phone.
- **Taken / turned down / missed**: yes / no / no answer before the ping wait ran out. Turned down and missed are recorded separately, but both move the ping to the next responder.
- **Ping wait**: how long a ping sits before counting as missed. Set in the release, the same for everyone. Routing config ships with releases and is not a runtime handler setting.
- **Acceptance rate**: share of pings *taken* (vs turned down or missed). Headline metric, reported weekly in aggregate. **Time-to-accept**: median seconds from ping sent to taken.
- **Coverage gap**: no available responder had the required capability tags. Counted separately from low acceptance, since a gap means nobody *could* go.
- **Routing priority**: score ranking responders. Inputs are proximity (travel-time estimate), availability, capability match and recent acceptance history. Turning down or missing a ping lowers recent acceptance, so lowers the responder's rank for later callouts.
- **Capability tags**: flight, structural-entry, hazmat-tolerant, cold-weather, aquatic, crowd-management, de-escalation.
- **Mutual aid**: responders covering for each other across areas. Not supported; on the Q4 exploration list.
- Supply terms: requisition, field failure report, service interval.

### Where things stand (as of the wiki's latest data)
- **Releases:** 4.0 (7 Apr: new nav, responder profile redesign, routing-override audit log). 4.1 (16 Jun: travel-time proximity, bulk callout, push reliability). 4.2 (12 Aug: proximity weighted up in routing, **ping wait cut 90s to 60s**, persistent console filters, three defect fixes).
- **The live problem:** since 4.2, fewer pings are being taken and more handlers are complaining. Two changes shipped together (routing weight and shorter ping wait) on top of a usually soft August. Priya's view is that it's mostly seasonal and will recover in September. That is her opinion, **not verified**. Test it against the data before accepting it. A mechanism worth checking: a shorter wait means more misses, and misses lower recent-acceptance, which lowers rank. Priya also said not to let this become a conversation about reverting 4.2, because proximity was a long-standing ask from wide-geography responders.
- **Q3 roadmap (all owned by Priya, last reviewed 30 Jun):**
  - Change to who gets pinged (4.2): shipped.
  - Ping timeout tuning (4.2): shipped as a cut to 60s. It's unclear whether that was "tuning" or a blunt change.
  - **Availability Confidence (4.2, Committed): not in the 4.2 release notes, so apparently squeezed out and still listed as Committed.** Priya says some items were squeezed out and that I need to agree with Helen which are still Q3 commitments. That conversation hasn't happened. The Q3 roadmap lists only one deferred item, so Priya's "a couple" is unaccounted for.
  - Requisition approval chains (4.3, Committed): a Supply item, listed against 4.3.
  - Handler phone app and Shared cover between responders (Q4, Exploring). Shared cover needs a close look against the confidentiality rule above.
- **Known noise:** console filter persistence will generate tickets. It's cosmetic, so don't let it eat the first month.
- **Gaps I'm expected to fill:** there is no written description of how we decide who gets pinged (Priya asked me to write it, working from Wen Li). Priya said she made calls faster than she checked them, so the least-examined parts of the product are the likeliest trouble.

### Working notes
- The running priorities list and open questions are in `00-rook/dispatch-priorities.md`. Read it at the start of a session and update its status boxes and log as work happens. Whenever you update that file, consider whether the task order should change; if so, show me a current-vs-proposed table in chat (new tasks flagged) and apply it only after I agree. Rank by impact, not by ticket or interview counts: weigh severity for each persona (responders, handlers, support, product, Supply users), breadth, what a task unblocks, and cost. The full criteria are at the top of that file.
- A rook-database connector is available for numbers. Rook-wiki search matches titles only; use read_page to see page text and comments.
- Treat the handover doc as one person's account, and wiki statuses as possibly stale (roadmap last reviewed 30 Jun).

### Learned in the Module 1 session (6 Oct 2026)
- The wiki root has three sections: Company (read), and Research and Product briefs (not yet read). Research holds Customer interviews and the 4.2 change page, and the first read of Research is still the best next step for the 4.2 evidence.
- `00-rook/code/dispatch-routing/` holds the routing code, a README and a CHANGELOG. They're unread, but they may be the missing written description of who gets pinged and may show what 4.2 changed. `00-rook/feedback/` is empty.
- No acceptance numbers appear in any source read so far, so every claim about the 4.2 drop (including Priya's "seasonal") is untested. rook-database is the likely source and hasn't been queried.
- Open: Availability Confidence missing from 4.2, no 4.3 release listed despite a "monthly" cadence, and no owner for the Supply roadmap item. The full list is in `00-rook/dispatch-priorities.md`.
- Evidence found later (6 Oct): support tickets (rook-database, 147 rows) went from about 0.9 a day before 4.2 to 4.0 a day after, with 15 about pings moving on before responders could answer and 30 about four responders (Farlight, The Undertow, Halfmoon, Corporal Ashgrove) getting few or no pings. None of either kind appear before 12 Aug, which contradicts Priya's "seasonal" view. All 30 of the latter are still open.
- The four September handler interviews (read in full) back this up: 3 of 4 describe pings moving on too fast, 2 of 4 describe quiet vs swamped responders (Kip's Meteor Mite vs The Gale), and 3 of 4 wanted alerts easier to notice. Halloran's complaints are Supply (requisition queue, failure reports, catalog search). Details are in `00-rook/dispatch-priorities.md`, now a single list ranked by impact.
- Still unread: the pings, callouts, handlers and responders tables, and the routing code. Checking pings per responder and the real ping wait is the next step.
- I asked for company context to be read narrowly (company folder and Company wiki section). Say plainly when a wider read is needed, and I'll ask before narrowing again.
- (Module 2) rook-database has five tables: support_tickets (147, no theme field, so themes in the priorities file are my keyword grouping and many tickets are templated), callouts (1,319, 29 Jun to 6 Sep), pings (callout_number, responder, sent_at, outcome of taken, turned down or missed), handlers (15) and responders. Pings and callouts are still unqueried.
- (Module 2) Tickets and interviews barely overlap: of the four interviewees only Mr. Ambrose filed a ticket, so ticket counts likely understate how many handlers see the ping problems. Tickets also can't show the swamped end of the skew (Kip's The Gale), and the handler alert requests may be a symptom of the shorter ping wait.
- (Module 2) The Farlight and The Undertow starvation is severe (about 12 to 1-6 pings a week) while Halfmoon and Corporal Ashgrove fell only about a third, so triage the first two. Supply tickets also hold safety signals (cracked vest plates, grapple line slow in cold, failure reports unanswered, medical notes cut at 250 characters), plus duplicate billing and a screen reader blocker.
- (Module 2) Prioritization now weighs persona severity, breadth, what a task unblocks and cost, not counts. A revised 28-task order (adds tasks for the swamped end of pings, interviewing handlers who filed no tickets, duplicate billing and screen reader support) was shown in chat but NOT yet applied. The priorities file still has the 25-task order. Ask whether to apply it.
- (Module 3) Release 4.2 effect, from the pings and callouts tables (29 Jun to 6 Sep; before = 29 Jun to 11 Aug, after = from 12 Aug): missed pings went from 2.3% to 18.0% of pings, a step on release day (weekly 1-3%, then 21.5%, 17.7%, 14.8%, 12.7%); turn-downs unchanged. The ping wait is live at 60s. Callouts per day also fell 15%, cause unknown, so raw pings-taken counts overstate the effect. Lead with the miss rate; 4.2's two changes (wait and weights) still cannot be separated.
- (Module 3) The four most starved responders by pings are Farlight, The Undertow, Vesper and Meteor Mite (about 7 to 2 pings a day combined). Vesper and Meteor Mite have no tickets (handlers Aunt Dot and Kip were interviewed), so tickets alone miss them; Halfmoon and Corporal Ashgrove, named in tickets, fell only about a third.
- (Module 3) Structural finding: when a callout's first ping goes to the responder in its own area and isn't taken, only about a third get a second ping (before and after 4.2); a non-local first ping always gets one. Fourteen of 15 areas have one responder. Why it stops is unexplained (ask Wen Li and Marcus). Also unexplained: 172 callouts per window whose first ping went outside the area (identical in a 44-day and a 26-day window), likely an artefact.
- (Module 3) Written this session: `00-rook/4-2-executive-summary.md` (observations framing, caveats at the top), `00-rook/charts/missed-pings-by-week.png`, `00-rook/data/` (callout-history.csv, unfilled-callouts.csv), tickets in `00-rook/feedback/tickets/`, briefs in `06-sidekicks/briefs/`. No Canvas tool exists in this setup.
- (Module 3) The task order in `00-rook/dispatch-priorities.md` WAS applied this session (23 active tasks, replacing the 25-task and 28-task orders mentioned above). Next: reply to starved responders' handlers, Marcus, Wen Li, then why a local unanswered ping ends the callout. Prior-year August data (Ravi) is still needed to test "seasonal".
- (Module 4) Routing code read (`00-rook/code/dispatch-routing/`). Each responder has one score from 0 to 1 (starts 0.5): a yes adds 0.08, a turn-down or a miss takes off 0.12 (same either way). Nothing in the code fades, resets or manually adjusts it, and it only changes when the responder is asked, so a low-ranked responder is frozen low and needs about 12 yeses in a row to climb from 0. 4.2 weights: proximity 0.60 (was 0.45), recent acceptance 0.25 (was 0.40), capability 0.15; wait 60s (was 90). The code keeps scores in memory; whether production saves them is unknown.
- (Module 4) Working hypothesis (`00-rook/starved-responders-hypothesis.md`): misses after 4.2 sank the four starved responders' scores, then their rank, so their own-area callouts went to others. Evidence: their miss rate is 53-64% vs 10-18% for peers; local first pings fell as a slide over about 3 weeks (Uptown 44 of 59 to 4 of 31, Harborside 50 of 63 to 8 of 32, Old Town 61 of 72 to 5 of 38). Before 4.2 every callout in those areas reached the local responder; now almost none do.
- (Module 4) Code and production differ: the code ranks only the callout's own region, but production pings neighbouring-area responders first (about 1 in 5 before 4.2, same neighbour pairs). That replaces the "172 callouts, likely an artefact" note. The README also omits the miss-lowers-score loop and the Supply dependency.
- (Module 4) Window test: turn-downs never take over 40s (median 23s), the four are not slower decliners, and misses are flat across the day. Yes-answer times are not in the data, so the shorter wait explains the general rise in misses but not why these four miss about 4x as often. To ask Wen Li and Marcus: live scores (prediction: four near 0-0.1, peers 0.9-1.0), time-to-answer for the four before 4.2, how the production candidate pool is built, whether scores persist across releases, and the override log (not in the database). Marcus's Slack question (did 4.2 apply to responders already turning jobs down?) has a drafted reply: it applied to everyone, and it hit people who started missing, not declining.
