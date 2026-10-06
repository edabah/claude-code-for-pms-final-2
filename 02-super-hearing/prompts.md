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
Did anyone say something that nobody else mentioned?

### 2.
What is the most alarming comments or themes from the interviews that I should focus on immediately that can be a major issue if not properly addressed

### 3.
Reading between the lines, is there an underlying cause for some of these issues that I should consider?

### 4.
Use the rook-database connector to read the support_tickets table. Same treatment as before: group them, tell me how many are in each group, and quote one line from each.

### 5.
You said the "quiet" tickets were only about "four responders"? How many responders are there in total? have other responders filed any tickets in similar time period? Help me understand if they are just "shy" and don't like to post tickets, or if there is likely no issue with them. I'm trying to understand the size of this issue

### 6.
So, are we seeing that requests "DROPPED" for these users, but INCREASED for other users who picked up some of their tickets?

### 7.
Only look at what came in after 12 August. What's different about these compared to everything before?

### 8.
You've now read both folders (interviews and tickets). Where do they disagree? What's loud in the interviews but rare in the tickets, and what's all over the tickets that nobody brought up in the interviews?

### 9.
If both piles are telling the truth, how can they both be right?

### 10.
So, are you suggesting that:

1. Volume was "slightly" down, although not enough to explain the issues
2. A code change made notifications disappear after 60s instead of 90s. That means, for the handlers that were "slower" to accept, their "acceptance rate" went down so they got fewer requests moving forward
3. Meanwhile, other handlers who accepted quicker got more requests as the system would route more to them


Or am I missing other points?

### 11.
If I only read interviews without tickets, what would I have thought?
If I only read tickets without interviews, what would i have thought?
