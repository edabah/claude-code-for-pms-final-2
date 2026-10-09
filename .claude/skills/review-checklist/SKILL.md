---
name: review-checklist
description: Reviews a product brief against a fixed seven-point checklist before it goes any further. Use it when the user says "review this brief", "run the review checklist", "check this brief", or points at a brief, PRD, one-pager or spec and asks whether it is ready to move on.
---

# Review checklist

Run the same seven checks, in the same order, on any brief the user points at. The user has already said what they look for, so don't ask them to explain it again.

## Before you start

1. Find the brief. Use the file or link the user named. If they named none, look for one `brief.md` in the current module folder; if there are several or none, ask which one.
2. Read the whole brief before judging any single check. Several checks compare one part against another.
3. Review only what is written. Don't fill gaps from memory or from other files, and don't invent owners, numbers or dates. If something is implied but not stated, count it as missing and say it is implied.

## The seven checks

Give each check one verdict: **Pass**, **Partial** or **Fail**. Quote or point to the line you based it on.

1. **Owner.** The brief names who owns it, a person (not a team or "TBD"). Partial if only a team is named.
2. **Users.** It says who faces the problem and who we are building for, specific enough to picture (for example "handlers entering incidents", not "users"). Partial if users are named but it is unclear whether they are the ones we are building for.
3. **Problem, described and quantified.** The problem is described in plain terms and backed by at least one real number (a rate, count, ticket volume, time lost). Partial if described but not quantified, or if the numbers have no source or date.
4. **Problem before fix.** The problem is explained before any solution is proposed. Fail if the fix appears first, or the problem section is written to justify a fix already chosen.
5. **Success metric.** It says how we'll know it worked, with a named metric, a baseline or current value, a target, and a time to measure. Partial if the metric is named but has no target or baseline. Fail for "improve satisfaction" style statements.
6. **Scope matches.** Compare the scope stated at the start (summary, goals, what's in and out) with the scope at the end (solution, deliverables, risks, next steps). Flag anything that appears at the end but not the start, or the reverse, and anything promised at the start that is never delivered. Pass only if they line up.
7. **Timeline.** It gives an overall project timeline: start, key milestones or phases, and an end or ship date. Partial if there are dates but no phases, or phases but no dates.

## Output

Use this shape and keep it short:

```
Brief: <title or file name>
Verdict: <Ready / Ready with fixes / Not ready>

| # | Check | Verdict | Evidence |
|---|-------|---------|----------|
| 1 | Owner | ... | ... |
...

Fix before it goes further:
1. <the most important fix, in one sentence>
2. ...
```

- **Ready**: all seven Pass. **Ready with fixes**: no Fail and at most three Partial. **Not ready**: any Fail, or more than three Partial.
- List fixes in priority order, most blocking first. Say what to add, not just what is wrong.
- Don't rewrite the brief and don't edit it unless the user asks. Offer to apply the fixes afterward.
- If the brief states a number, date or owner you can't confirm, don't judge whether it is true. Note that you only checked that it is present and sourced.
