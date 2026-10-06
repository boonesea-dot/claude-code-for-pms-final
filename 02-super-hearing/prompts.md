# 02 · Super Hearing — prompts

**Context:** You joined Rook two weeks ago as PM on Dispatch.
Release 4.2 shipped on 12 August, just before you arrived, and
landed badly. Last session you built a context file and met the
company.

You still do not know what actually went wrong.

At the end of the session, type wrap up, and Claude Code saves the
prompts you wrote yourself below — not the starter prompt — updates
CLAUDE.md, and saves your work to GitHub. By Module 6 this file is a
prompt library built from your own questions.

---

### 1.

Pull support tickets from the Rook database, group them by theme and give me a count of issues, and delineate issues from before and after the 4.2 release. Compare those themes to our list of contradictions/omissions, and our task list .md file so that we can see if any new tasks are needed, or if we have additional context to add to existing tasks to aid in resolving them

### 2.

Our task list is growing, and we have added a lot of context from the initial creation of the dispatch-priorities.md file. Review what we have now, and restructure the priority list so that items are arranged in descending order based on the impact resolution will have. We will likely repeat this exercise in the future, so if you find yourself updating this .md file, ensure that every time you do so, we consider reprioritization.

However, do not automatically update the file with the change in order. Instead provide in chat to me a table with the current order and proposed new order side by side, with any new tasks being added clearly identified.

### 3.

yes, the order you have looks good, and let's apply the new guardrails to the .md file, as well as the restructuring into a single list of tasks rather than a weekly breakdown

### 4.

We should pull out some quotes from the support tickets as we did with the interviews to establish themes. 

Also when considering prioritization, be sure to not simply weight higher occurrences as higher priority. Consider also the impact to a persona using the system (both end user, support, product, etc.) of a given item as well as the "item 1 unblocks item 2" logic you referenced already

### 5.

In the exercise I'm completing, I've worked a bit ahead, I need to also run this prompt " You've now read both folders (interviews and tickets). Where do they disagree? What's loud in the interviews but rare in the tickets, and what's all over the tickets that nobody brought up in the interviews?" and let's see what insights that gives on what we've already done looking at both data sources
