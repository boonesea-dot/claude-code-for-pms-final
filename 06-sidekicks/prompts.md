# 06 · Sidekicks — prompts

**Context:** You joined Rook two weeks ago as PM on Dispatch.
Release 4.2 shipped on 12 August, just before you arrived, and
landed badly. You've spent five sessions on it: finding out what
went wrong, then building the fix nobody had — a working prototype,
by hand, in one sitting, for one room.

At the end of the session, type wrap up, and Claude Code saves the
prompts you wrote yourself below — not the starter prompt — updates
CLAUDE.md, and saves your work to GitHub. By Module 6 this file is a
prompt library built from your own questions.

---

### 1.

I want to save a habit as something I can reuse. When I review a brief before it goes any further, here's what I actually check for:

1. It names who owns it
2. It says how we'll know it worked
3. The scope at the end matches the scope at the start
4. It explains the problem before it proposes a fix
5. Claims are distinguished from assumptions
6. The proposed solution follows from the problem
7. The target user and context are explicit
8. Dependencies and system boundaries are identified

Turn that into a skill called review-checklist, so I can point it at any brief and get the same check every time, without me explaining it again.

### 2.

/review-checklist Let's run the skill against the brief.md file in 05-super-speed

### 3.

I own the brief, let's add the other responders, Live scores do show the four near zero. Let's create the "also happens" section, and set 7 days for the initial value for 'n' but state that it's subject to change

### 4.

And yes, the prototype you see in the .html file exists, so the brief is stale

### 5.

/review-checklist

### 6.

Is there context in our other chats from which we can draw the live scores? Otherwise as this is a training exercise, for the purpose of the check, let's add something so the checks in the skill pass. We also added an admin tool to reset responders to 0.5 in the prototype, does that answer item 1? Maybe factor that in, otherwise  your proposed example makes sense

### 7.

/review-checklist

### 8.

Tidy the loose ends

### 9.

push and commit

### 10.

/review-checklist run the skill against https://github.com/RMThomas77/claude-code-for-pms-final/blob/main/05-super-speed/brief.md

### 11.

Schedule review-checklist to run every Monday morning, and let me know what it finds. Nothing needs to be ready for it to fire today. I'm setting the habit, not waiting on the result.

### 12.

Tweak this so it only reviews briefs in this project folder
