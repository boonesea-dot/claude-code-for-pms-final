# 04 · X-Ray Vision — prompts

**Context:** You joined Rook two weeks ago as PM on Dispatch.
Release 4.2 shipped on 12 August, just before you arrived, and
landed badly. Last session the numbers showed you what happened: a
ping used to wait ninety seconds and now it waits sixty, people
missed pings they used to catch, and missing one counts the same as
turning one down — so four responders stopped hearing from us
altogether.

At the end of the session, type wrap up, and Claude Code saves the
prompts you wrote yourself below — not the starter prompt — updates
CLAUDE.md, and saves your work to GitHub. By Module 6 this file is a
prompt library built from your own questions.

---

### 1.

With what we know about the problems Farlight and others have seen/reported both predating and following the 4.2 release, inspect for gaps in the code and in chat describe what those gaps are, and what the impact of its absence is

### 2.

Are there inconsistencies between what you see as these gaps in the code and things the readme indicates should be there?

### 3.

From our current understanding of this situation from tickets, interviews, and analysis as well as the code, propose a hypothesis as to the reason why some responders are not getting pings that I can share with engineering, Wen Li, and Marcus for validation

### 4.

Yes and let's distill this down to fit the format  "Based on what I found, the reason some responders are getting no pings at all is ______, because ______.

### 5.

There was a message in a team Slack channel from Marcus "Quick one, did the change to who gets asked first apply to responders who'd already been turning jobs down, or just new ones? Can't tell from the code and don't want to guess on this. Anyone know?"

I'd like to answer his question, while keeping in mind our hypothesis against the data. Are his question and our hypothesis related?

Provide me a conversational, slack friendly one line response in chat, as well as your assessment of his question and relation to our hypothesis

### 6.

Can we uncover anything else in our source data, the code, or other context that might bring clarity to the mismatch between code and prod as far as ranking and why out of region responders are being pinged first?

### 7.

Pull in a couple of real world examples from historic data to support this, and let's reformulate our response to Marcus

### 8.

Help me understand a bit more from the code about how the scores for these responders are being tracked. This seems like an important variable in what we are seeing, and I need to understand it better

### 9.

Find me the part of this code that takes points off somebody when they miss a ping or turn one down. Show it to me and explain it in plain English. Then find me every single thing in this code that puts points back on.

### 10.

So essentially once someone hits the floor, or at least drops sufficiently there seems to be no way short of manual intervention on their ranking for them to recover?

Once at 0, they remain there because the logic skips them entirely, and if their score is low enough, and there are other responders in a proximity such that it would increase their weight, then they get skipped over, which drives them closer to the floor?

### 11.

but there is a compounding effect based on the shortened ping time, where a responder like Farlight who only gets one ping, and now has a shorter time to respond has an increased risk of not being able to climb out of that hole. It's not impossible, but when you have one opportunity to increase your score per week, if you miss it because the window to respond is tight (and secondarily because a miss is treated the same as a decline) it is very hard

### 12.

Can we perform that test?

And as a secondary ask what about that "recent" gap you mentioned where there is no fading over time? Seems like another gap to explore

### 13.

Let's roll this up into a concise summary that fits the format "According to my findings in the code, for someone who's gone quiet, they would need to ___."
