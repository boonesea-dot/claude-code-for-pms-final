# Rook Dispatch: priorities and open questions

Living file. Update the status boxes and the log as work happens. Created 2026-10-06 from the handover doc and the rook-wiki Company section. Nothing here is verified against data yet; the rook-database and Research pages have not been read.

Status key: `[ ]` open, `[~]` in progress, `[x]` done

## Week 1: establish the facts

- [ ] **1. Find out whether acceptance is down, and why.** Priya's "seasonal, back in September" is untested.
  - Ask Ravi Menon for weekly acceptance rate and time-to-accept from before 4.2 (12 Aug) to now, with turned-down and missed split out and a by-geography cut. Also prior-year Aug/Sep for the seasonal check.
  - Check whether rook-database has this directly.
  - Decide: routing change, 60s ping wait, or season? Can the two be separated by date or segment?
- [ ] **2. Meet Wen Li on how ranking works.** She is the only source. Get the routing score inputs, old vs new weights, and what a miss costs a responder's rank.
- [ ] **3. Talk to Helen Achebe about commitments.** Not yet had, per Priya.
  - Which Q3 items are still commitments (Availability Confidence is Committed for 4.2 but not in the release)?
  - Was Ping timeout tuning "tuned", or just cut from 90s to 60s?
  - Who owns Requisition approval chains (4.3)? What is the release cadence now?
  - What does she want from me in the first 30 days?

## Weeks 1-2: gather evidence

- [ ] **4. Read the September interviews** (Research > Customer interviews) and talk to Sofia Marino.
- [ ] **5. Standing 15 minutes with Nadia Hoffmann.** 4.2 ticket volume by theme; separate routing complaints from console-filter noise.
- [ ] **6. Check with Marcus Oyelaran** on the current state of 4.2, available telemetry, anything shipped in August not in the notes, and whether ping wait can be tested separately from routing weights.

## Weeks 2-4: fix the gaps

- [ ] **7. Define the 4.2 problem and a decision date.** State the question and what we'd do under each answer. Options short of a full revert, e.g. reverting only the ping wait or varying it by responder group.
- [ ] **8. Fix the acceptance metric.** Split turned-down from missed; report by geography.
- [ ] **9. Write the ranking description** (Priya's ask) from item 2; put it in the wiki next to the Glossary.
- [ ] **10. Refresh the roadmap.** Last reviewed 30 Jun, all items owned by Priya, Q3 is over. Re-own, mark shipped/slipped, set what's next.

## Lower priority

- [ ] 11. Ask the Supply PM whether they saw changes to maintenance scheduling after 4.2 (Dispatch writes the Responder Availability Record; Supply reads it).
- [ ] 12. Find and read Security Policy 4.1 (not in the wiki). Do this before looking at Shared cover between responders.
- [ ] 13. Pick one term, "ping wait" or "ping timeout", and use it everywhere.
- [ ] 14. Read the Routing Override Audit Log page; a rise in handler overrides after 4.2 would be a signal.
- [ ] 15. Q4 exploration items (Handler phone app, Shared cover, mutual aid) only after the 4.2 question is settled.

## Contradictions and gaps in the docs

- **Cadence:** docs say monthly releases; actual dates are 4.0 (7 Apr), 4.1 (16 Jun), 4.2 (12 Aug). Nothing for 4.3, though Requisition approval chains is committed to it.
- **Ping wait:** a committed roadmap item ("Ping timeout tuning") but described in the handover as incidental. Also two names for the same thing.
- **Squeezed items:** Priya says "a couple"; the roadmap shows only Availability Confidence still unshipped.
- **No numbers anywhere:** no acceptance, time-to-accept, miss rate or ticket counts, before or after 4.2.
- **Metric design:** acceptance rate lumps turned-down with missed, and is reported only in aggregate.
- **Ranking:** no documented weights or before/after values for the 4.2 proximity change.
- **Roadmap:** stale (30 Jun) and all items owned by a departed PM. Requisition approval chains is a Supply item with no known Supply owner.
- **Mutual aid:** Glossary says it's on the Q4 exploration list; the roadmap database doesn't list it.
- **Security Policy 4.1** is referenced in About Rook but isn't in the wiki.
- **Wen Li** was away 14-24 Aug, covering the 4.2 release and the first complaints. Who reviewed the routing change?

## Pages not yet read

Research (Customer interviews, Change to who gets pinged), Product briefs, Handler Phone App, Bulk Callout, Requisition Approval Chains, Routing Override Audit Log, the four handler pages (Aunt Dot, Mr. Ambrose, Halloran, Kip), page comments (except two roadmap rows), and rook-database.

## Log

- 2026-10-06: File created. No items started.
