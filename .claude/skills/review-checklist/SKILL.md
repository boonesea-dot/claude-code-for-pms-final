---
name: review-checklist
description: Reviews a product brief against a fixed 8-point checklist (ownership, success measure, scope consistency, problem before solution, claims vs assumptions, solution fit, target user, dependencies). Use when the user asks to review a brief, run the review checklist, or check a brief before it goes further.
---

# Review checklist

Run the same eight checks on any brief, every time. Review only; do not edit the brief.

## Input

The user points at a brief (file path, pasted text, or a wiki/doc page). If none is given, ask which brief. Read all of it before judging anything, including the end, since check 3 compares the end to the start.

## The eight checks

For each check, give a verdict of **Pass**, **Partial**, **Fail**, or **N/A** (N/A only with a one-line reason), and back it with a short quote or a section reference from the brief. A verdict without evidence is not allowed.

1. **Owner named.** A named person (or team with a named lead) owns the brief and the outcome. "We" or "the team" does not count.
2. **Success measure.** It says how we will know it worked: a metric or observable outcome, a baseline or target, and when it will be checked. A goal with no way to tell it was met is Partial at best.
3. **Scope matches.** The scope stated at the start matches what the end commits to. Look for items added or dropped in the solution, asks, next steps, or timeline that the opening scope never mentioned.
4. **Problem before fix.** The problem is explained, with who has it and what it costs, before any solution is proposed. Flag solutions that appear first or problems that are only implied.
5. **Claims vs assumptions.** Statements backed by evidence (data, research, a named source) are distinguishable from assumptions and opinions. Flag unsourced statements written as fact, and say which sentences you mean.
6. **Solution follows from problem.** The proposed solution addresses the stated problem and its cause. Flag fixes that solve a neighbouring problem, skip a step in the reasoning, or ignore a plausible alternative cause.
7. **Target user and context.** The target user is explicit (who, in what situation, using what). Flag "users" in general, or several user types treated as one.
8. **Dependencies and boundaries.** Other teams, systems, data, or commitments this touches are identified, along with where this work stops and theirs starts. Flag anything the proposal would change in another system without saying so.

## Output

Use exactly this shape so reviews are comparable:

```
# Review: <brief title>

**Result:** <n> Pass, <n> Partial, <n> Fail, <n> N/A
**Ready to go further?** <Yes / Not yet / No>, <one sentence why>

| # | Check | Verdict | Evidence |
|---|-------|---------|----------|
| 1 | Owner named | ... | ... |
| ... | | | |

## What to fix first
1. <highest-impact gap, and what would close it>
2. ...
(Only Partial and Fail items. Order by how much each blocks the brief from being trusted.)

## Questions for the author
- <specific questions the brief cannot answer>
```

"Ready" means Yes only if there are no Fails and no Partials on checks 2, 4, or 6. Otherwise "Not yet", or "No" if the brief has three or more Fails.

## Rules

- Judge only what is written. Do not fill gaps with outside knowledge or guess the author's intent; a missing item is a finding.
- Be specific: quote the line, name the section. Keep each Evidence cell to one or two sentences.
- Do not rewrite the brief or suggest new content beyond what closes a gap. Offer to help fix it afterwards.
- Do not skip a check because the brief is short or informal. A missing section is a Fail on that check.
