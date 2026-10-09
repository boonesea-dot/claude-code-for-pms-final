# 05 · Super Speed — prompts

**Context:** You joined Rook two weeks ago as PM on Dispatch.
Release 4.2 shipped on 12 August, just before you arrived, and
landed badly. You've spent three sessions finding out what went
wrong: the two piles of feedback that didn't agree, the numbers that
hid four people inside an average, and the code that settled it —
the shorter timeout is what did it.

At the end of the session, type wrap up, and Claude Code saves the
prompts you wrote yourself below — not the starter prompt — updates
CLAUDE.md, and saves your work to GitHub. By Module 6 this file is a
prompt library built from your own questions.

---

### 1.

I received a message from Helen, which you can review at 05-super-speed/director-request.txt for context. 

Let's start working through that brief. Don't worry about keeping it tight to one page as a first draft, let's make sure we have all the context we need, and the brief covers 3 things, which we'll tighten up to one page as we iterate.

1. Declare who the solution is for - Let's use Farlight as our example from all the work we did yesterday
2. What changes for them once the fix exists
3. What the fix deliberately doesn't do

### 2.

Let's move forward with the assumption that our hypothesis on cause is correct, and redraft with that in mind, but do mention this is based on a hypothesis - We want the draft to be "what we would do if we're right"

Similarly for Wen's 2019 note - Let's assume in our fix we have the scores fade back. To me it seems this enhances the ability of a responder to recover moving forward, and ensures a stretch of misses doesn't remain with them indefinitely. 

Is there anything else we should revisit based on the weighting of out of area responders and the distance factor in the score starving responders in the immediate area?

### 3.

The one other item I'd like to probe before tightening up the draft is the observation you made about the link to supply that we found yesterday. Does that bear treating as another out of scope item for this fix, but a fast follow, or open question?

### 4.

Yes, and let's tighten to a final one page draft

### 5.

That looks tight enough, any way to add a visual aid (e.g. graph(s))? I think if we keep them small we'll still land within the one page

### 6.

Can we state the fix more prominently in the document, maybe pull it out of section two and put it above section one?

### 7.

Save the brief exactly as it stands now as 05-super-speed/brief.md. Show me the file when it's done.

### 8.

Take the brief you just wrote and build me a working prototype, an actual screen I can click through, not a description of one. Save it as 05-super-speed/prototype.html, a single file I can just open in my browser. Show me where this would actually happen, and let's start by making at least one thing on it respond when I click it.

### 9.

Let's add the phone view

### 10.

Can we make it so the responder taking a ping via phone view also flows back to console view and affects the score and graphs? We'd need a reset option as well (probably in the console view) to restore to the baseline to demo again

### 11.

The flag as quiet slider affects only when the status switches, right? We're not adjusting the half life for score recovery?

### 12.

We talked about providing immediate relief for the starved responders as well, in the prototype let's add an "admin" tool which we can use to reset Farlight to the neutral score to visualize that affect. It should affect the graph, and after a reset, if they remain quiet , we should see the score drop towards zero if we move the days since fix slider
