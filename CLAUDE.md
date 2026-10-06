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
- The running priorities list and open questions are in `00-rook/dispatch-priorities.md`. Read it at the start of a session and update its status boxes and log as work happens.
- A rook-database connector is available for numbers. Rook-wiki search matches titles only; use read_page to see page text and comments.
- Treat the handover doc as one person's account, and wiki statuses as possibly stale (roadmap last reviewed 30 Jun).

### Learned in the Module 1 session (6 Oct 2026)
- The wiki root has three sections: Company (read), and Research and Product briefs (not yet read). Research holds Customer interviews and the 4.2 change page, and the first read of Research is still the best next step for the 4.2 evidence.
- `00-rook/code/dispatch-routing/` holds the routing code, a README and a CHANGELOG. They're unread, but they may be the missing written description of who gets pinged and may show what 4.2 changed. `00-rook/feedback/` is empty.
- No acceptance numbers appear in any source read so far, so every claim about the 4.2 drop (including Priya's "seasonal") is untested. rook-database is the likely source and hasn't been queried.
- Open: Availability Confidence missing from 4.2, no 4.3 release listed despite a "monthly" cadence, and no owner for the Supply roadmap item. The full list is in `00-rook/dispatch-priorities.md`.
- I asked for company context to be read narrowly (company folder and Company wiki section). Say plainly when a wider read is needed, and I'll ask before narrowing again.
