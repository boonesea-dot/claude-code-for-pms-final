# 03 · Rewind — prompts

**Context:** You joined Rook two weeks ago as PM on Dispatch.
Release 4.2 shipped on 12 August, just before you arrived, and
landed badly.

Last session you read four conversations and every support ticket
since 4.2 — and found the two piles did not agree.

At the end of the session, type wrap up, and Claude Code saves the
prompts you wrote yourself below — not the starter prompt — updates
CLAUDE.md, and saves your work to GitHub. By Module 6 this file is a
prompt library built from your own questions.

---

### 1.

open 00-rook/data/callout-history.csv. every row is one responder in one week: how many times we pinged them, and how many of those they took. Release 4.2 shipped on 12 August. Tell me what changed after that  date. Show me the weekly numbers before and after, and show me the rows you used to get them

### 2.

By 'the file' I assume you mean the metrics report. Is there data in another data source that we can use to fill in that gap for the ping wait or routing weight? I'm going to be asked to report to a Director of Product about what release 4.2 actually did to the responders, and I need to present as accurate of an answer as I can

### 3.

Analyze the data, and look for any anomalies in terms of pings sent. We know that there are fewer pings picked up, especially by the starved 4, while others have somewhat picked up the slack. But if we are sending fewer pings, any drop in pickup is more substantially weighted

### 4.

Let's roll up what we've uncovered into an executive summary that I can share. Open it in Canvas so I can propose edits and we can revise as we move on. Pull the callout text and save that for me in our working folder. Additionally, other than discussing the callout volume with Marcus give me an assessment of other next steps, and let's bump that up against our task list and go through prioritization/update

### 5.

The .md executive summary is fine. As for the contents, I'd make some adjustments. Let's frame this from the perspective of what we are observing, with supporting facts as we have them. Move caveats further up the document as they are important for understanding the context and would be helpful to know before reading too far

### 6.

Let's keep the executive summary, but in chat, give me one number to share that would be most impactful for Hellen as the Director of Product from what we've found

### 7.

Going back to your comment "

Callout volume fell, and that changes how to read the drop in pickups. Total pings sent fell only about 5%, but callouts fell about 15%. Most of the 20% fall in pings taken follows the fall in callouts, not a worse hit rate.
I compared 29 Jun to 11 Aug (44 days) with 12 Aug to 6 Sep (26 days), using per-day rates so the unequal windows don't distort anything.
 
Per day	Before	After	Change
Callouts	20.0	16.9	-15%
Pings sent	24.7	23.5	-5%
Pings taken	18.9	15.0	-20%
Pings per callout	1.23	1.39	+13%
Callouts where no one took it	5.5%	11.1%	doubled

"

Cite your source(s)

### 8.

restate your points about areas with one responder and the risk

### 9.

Sanity check - Comparing the tickets against the datafile from the metrics report, does anything change about our observations or next steps. To me the one liner about missed pings is what to lead with (though a graphic to go with it would be nice)

Beyond that, we have our task list of items to follow up on, and to me the observation of the impact of missed pings on regions with a single responder is probably something that we want to track down as a root cause sooner rather than later in terms of our task list

### 10.

Let's update the task order and add this to the summary
