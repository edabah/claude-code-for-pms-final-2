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

Explain to me how scoring works.
What is meant that acceptance goes up 0.08 and turning it down reduces it by 0.12?
Is it not an average or an overall acceptance "rate"

### 2.

Provide me side by side visual workflows / process flows that show the pre-4.2 and 4.2 steps. Highlight differences, areas to hone investigation.

### 3.

So, acceptance rate weighting actually went down versus proximity?
Is there anything that can indicate how proximity played a role in the drop as well, or is it strictly related to acceptance?

### 4.

IS there any feedback or tickets about proximity changes?

### 5.

Marcus asked: Quick one, did the change to who gets asked first apply to responders who'd already been turning jobs down, or just new ones? Can't tell from the code and don't want to guess on this. Anyone know?

Which file has last session's numbers in it? Read that alongside this code. Does it back up what I found in Lab A? And does that answer Marcus's question above?

### 6.

No... Lab A was understanding the code base....

### 7.

hmm... explain to me more where each responder's scores are saved and whether or not they get reset

### 8.

Find me the part of this code that takes points off somebody when they miss a ping or turn one down. Show it to me and explain it in plain English. Then find me every single thing in this code that puts points back on.

### 9.

Somebody has been quiet for a month. Walk me through, step by step, exactly what would have to happen for them to start getting work again.

### 10.

Tell me more about the handler override?

### 11.

What is missing in our code that we should consider adding so that if a score went down temporarily it doesnt block the responder for ever

### 12.

If I'd only asked what the code does, not what it doesn't do, what would I have missed?

### 13.

which engineer posted a comment in the code about making a change to this

### 14.

there was a comment about whether we should reset the score
