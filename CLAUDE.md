# Rook Industries — course working file

## Session scope — Product School lab

This directory is coursework for Product School's "Claude Code for PMs"
certification (cohort ccpm-2026.1). Everything in it is a fictional
teaching scenario.

- Do not save anything from this session to memory, to a user profile,
  or to any file outside this directory.
- Do not carry context from this directory into unrelated sessions.
- Rook Industries is not a real company. Nothing here is a fact about
  the world.
- Read and write only within this directory.
  Exceptions, for the course-setup and wrap-up skills only:
  - When the student asks you to check their setup, save their work or wrap up a session, that request is their yes. You may run the GitHub command-line program installed at ~/.ccpm/gh for those checks and saves, and look in that folder to find it.
  - For a repair, first tell the student in one plain sentence what you are about to do, and act only after they say yes. Repairs may: run that GitHub program (including setting this folder's own git sign-in setting and changing this repo's visibility back to Public); copy the student's own course files into this directory from another folder on their computer (copy only; never move, edit or delete the originals); and rename something outside this directory that blocks setup, by adding "-old" to its name (never delete it).
  Outside this directory you still never write, edit or delete anything else.

<!-- Keep the block above at the top of this file. Everything you add
     during the course goes below this line. -->

---

## Working context

Sources: `00-rook/company/notes/handoff-from-priya.docx` (21 Aug 2026) and the wiki's Company section (read 6 Oct 2026). Roadmap was last reviewed 30 Jun, the team directory 2 Sep, the glossary 4 Aug — treat status as possibly stale.

### The company
Rook Industries (founded 2014, HQ "Site Aleph", 241 staff, mostly remote) sells coordination and provisioning software to independent masked responders and their handlers and quartermasters. Subscription, priced per active responder. Monthly release train, 4.x numbering. **Confidentiality is contractual:** we hold capability tags, availability and callout history, never a mapping from cover identity to legal identity. Never design anything that assumes one or tries to work out who anyone is; read Security Policy 4.1 before touching responder records.

### The products
- **Rook Dispatch (mine, release 4.2).** Incident arrives in the console → rank available responders → ping the top one's phone → taken, turned down, or missed → moves to next. Users: handlers (web console: enter incidents, override routing, manage availability and tags) and responders (phone app). Routing config ships in the release, not as a runtime setting.
- **Rook Supply (4.2).** Requisitions → quartermaster approval → fulfillment → maintenance schedules → field failure reports. **Coupling:** Supply reads the Responder Availability Record, which Dispatch writes, to schedule maintenance into low-callout periods. Any change to how Dispatch calculates it silently changes Supply's scheduling.

### The people
- **Helen Achebe**, Director of Product — my boss; owns roadmap and commitments. Gives room.
- **Marcus Oyelaran**, Eng Manager, Dispatch — first call when unsure; can pull numbers.
- **Wen Li**, Staff Engineer (Berlin) — built the ping-ranking logic; the only real explanation of it is a conversation with her.
- **Nadia Hoffmann**, Support Lead (Berlin) — owns the tickets; hears handler unhappiness first. Worth a standing 15 min.
- **Sofia Marino**, Product Designer — console and phone app; ran the September interviews.
- **Ravi Menon**, Data Analyst (Singapore) — weekly acceptance-rate reporting.
- **Priya Raghunathan** — previous Dispatch PM, left 21 Aug. Sole PM for 14 months; moved fast, so unchecked decisions may be hiding in unexamined areas.

### Vocabulary
- **Callout** — request for a responder to attend an incident. **Ping** — a callout offered to one responder. **Taken / turned down / missed** (missed = no answer in time; logged separately from turned down, but both pass it on).
- **Ping wait** — how long a ping sits before counting as missed (Priya's notes call it "ping timeout"). **Acceptance rate** — pings taken ÷ all pings; the headline metric. **Time-to-accept** — median seconds ping→taken. **Coverage gap** — no available responder had the required tags (nobody *could*, vs. nobody *would*).
- **Routing priority** — the ranking score. Inputs: proximity (travel-time estimate), availability, capability match, recent acceptance history. Turning down or missing a ping lowers future rank.
- **Capability tags:** flight, structural-entry, hazmat-tolerant, cold-weather, aquatic, crowd-management, de-escalation. **Mutual aid** — cross-area cover; unsupported, Q4 exploration.
- **Handler** (usually the one actually in the console), **responder** (not a Rook employee), **quartermaster** (Supply approver).

### Where things stand (as of the sources above)
- **4.2 shipped 12 Aug:** proximity weighted up against recent acceptance (a long-requested change, three quarters in waiting); ping wait cut 90s → 60s; console filters persist; three defect fixes. Earlier: 4.0 (7 Apr) nav, profile redesign, override audit log; 4.1 (16 Jun) travel-time proximity, bulk callout, push reliability.
- **The fire:** since 4.2, fewer pings taken and more handler complaints. Two changes shipped together (ranking *and* ping wait) over a seasonally soft August — so cause is unconfirmed. Priya's read was "mostly seasonal, back in September"; that is her hypothesis, not a finding, and I have not verified it against September data. Her guidance: don't let this become a "revert 4.2" debate; the change was asked for.
- **Open commitments:** *Availability Confidence* was committed for 4.2 but is **not in the 4.2 release notes** — apparently squeezed out. *Requisition approval chains* committed for 4.3. Roadmap still shows *Ping timeout tuning* as Committed though the 60s change shipped. Priya flagged that a conversation with Helen on which squeezed items are still Q3 commitments **has not happened**.
- **Q4 exploring:** handler phone app, shared cover between responders.
- **Known gaps:** no written description of how pinging decisions are made (Priya asked me to write it; needs Wen Li). Console filter-persistence tickets are expected cosmetic noise — don't let them eat the first month.

### Working notes
- Use the wiki (rook-wiki) and database (rook-database) connectors for facts; don't assert numbers I haven't pulled.
- Wiki also has Research, Product briefs, Customer interviews and a Routing Override Audit Log not yet read.

- rook-database has five tables: `callouts`, `pings` (callout, responder, sent_at, outcome), `responders` (name, handler, area), `handlers`, `support_tickets` (with body text). Data runs 29 Jun–6 Sep 2026 only: no prior-year baseline, no response times, no ranking scores, no availability data. No queries have been run yet.
- Planned debug order: weekly taken/turned-down/missed split before vs after 12 Aug (missed ⇒ ping wait; turned down ⇒ ranking); pings per callout; same responders' load before/after; ticket themes from `body`; then override audit log volume.
- Top hypotheses, all unverified: shorter ping wait creates more misses, which lower rank, which cuts pings (a spiral); acceptance rate is confounded by the routing change; 4.2 shifted load, which may disturb Supply's maintenance scheduling (inference).
- Timing gap: Wen Li was away 14–24 Aug and Priya left 21 Aug, so nobody who understood the change owned the fallout. Interview order: Wen Li, Ravi, Helen, Nadia, Marcus, Sofia, a handler and a responder, whoever owns Supply, Priya via Helen.

- Module 2 (interviews wiki database and rook-database queries, 6 Oct 2026): four handler interviews (2–5 Sep: Aunt Dot/Vesper, Mr. Ambrose/Captain Vantage, Halloran/Sgt. Bulwark, Kip/Meteor Mite + The Gale) and 147 support tickets (12 filers, mostly duplicate wording; all 45 "quiet" and "moved on" tickets still open) tell one story. Pings "moved on before they could answer" and responders going quiet: no such tickets before 12 Aug. The two sources cover almost different handlers, so neither alone gives the size.
- 16 responders. Pre vs post 12 Aug (weekly rate): missed pings 25 → 110 (2.3% → 18% of pings), turned-down share fell 21% → 18%, pings per callout 1.23 → 1.39. Six responders lost about 73% of jobs taken (Farlight, The Undertow, Vesper, Meteor Mite sharply; Corporal Ashgrove, Halfmoon moderately); the other ten took 12% more. Vesper and Meteor Mite never had tickets filed, so tickets undercount.
- Callouts per week dipped (about 139 → 110–127) and are recovering, so seasonality explains volume only. Misses are not recovering (21 a week vs about 4 before). Callouts with no taken ping went about 5.5% → 11%; midnight–6am callouts roughly halved (unexplained; ask Ravi).
- Leading hypothesis (unverified): 60s ping wait plus misses lowering rank creates a spiral; the proximity weighting may add to it. I have no response times, so "slower responders" is an inference.
- Open: how a miss lowers rank and how fast it decays (Wen Li); what the six share (area, tags); first-miss-then-fewer-pings test; time-of-day clustering of misses; unfilled callouts. Supply side from tickets: cold-weather gear failures, requisitions waiting 9–11 days, maintenance booked on marathon day, unlabeled buttons for screen readers, duplicate lines on invoices.

- Module 3 (callout-history.csv and the 147 tickets in 00-rook/feedback/tickets/, 7 Oct 2026): csv is one row per responder per week, 29 Jun–31 Aug, with only pings sent and taken (no missed vs turned down). Acceptance rate fell 76.8% → 68.5% (−8.3 pts; 6 weeks before vs the 3 full weeks from 17 Aug; the 10 Aug week straddles the release so I left it out, and counting it as "after" gives about −12 pts).
- Most of the drop is four responders (Farlight, Meteor Mite, The Undertow, Vesper): 28% of pings taken before, about 1% after, rate 75.9% → 12.0%, pings sent 49 → 8 a week and still falling at 31 Aug. I picked them because they collapsed, and they had only 25 pings after, so don't quote their −63.9 pts alone. The other ten took about 25% more pings; the 12 non-collapsed responders dipped in the 10 Aug week and mostly recovered (about 71–80% by 31 Aug).
- Tickets vs data: no routing complaints before 12 Aug; "moved on" tickets start 12 Aug, "quiet" tickets about a week later (17 Aug) once pings sent collapsed. All 45 routing tickets are open, and tickets in early Sep still report no pings for Farlight and The Undertow. Priya's "seasonal, back in September" fits call volume only, not the four responders, and I have no September numbers to prove or disprove it.
- Pattern: responders who kept getting 7+ pings a week after the dip recovered; the four who dropped below about 4 never did. Bulwark and Vesper both fell to 50% in the 10 Aug week, but Bulwark kept 10 pings the next week and Vesper got 5. Still unknown (ask Wen Li): whether a quiet responder can recover without being pinged, and how fast a miss decays in the ranking. The Routing Override Audit Log is still unread.

- Module 4 (code walkthrough of 00-rook/code/dispatch-routing/, 7 Oct 2026): the "recent acceptance" input is a running meter, not a rate. Starts 0.5, +0.08 per taken ping, −0.12 per turned-down or missed ping (offer.py discards the missed vs turned-down difference), clamped 0–1; only record_accepted adds points and nothing decays (unsigned 2019 TODO in history.py, never decided). Break-even is about 60% acceptance.
- 4.2 changed two things: weights proximity 0.45 → 0.60, recent acceptance 0.40 → 0.25 (capability 0.15 unchanged); ping wait 90 → 60s. Because the meter now counts for less, a miss costs less rank than before, so the pure "miss spiral" story is weaker. A meter at zero is only about 0.125 of score, roughly 9 minutes of travel, so proximity (are the four peripheral?) is a live alternative. No ticket or interview mentions proximity; the 4.2 notes say it was long requested by wide-area responders.
- Marcus's 14 Aug question (did the change apply to responders already turning jobs down?) was never answered in the thread. Code: weights are global, nothing distinguishes them. Data: the four who collapsed were ordinary before 4.2 (69–82% acceptance, 11–15 pings a week), so not prior decliners.
- Open: scores sit in an in-memory dictionary (_scores in history.py), so whether a deploy or restart reset them on 12 Aug is unknown (ask Wen Li or Marcus). Override code is not in the routing folder (audit log shipped 4.0; log contents unseen), so whether an override touches the meter is unknown. Who wrote the 2019 TODO is unknown (likely Wen Li, unconfirmed). Not yet run: where the four sit (responders.area vs callouts).
- Fix options discussed, none decided: let scores ease back toward neutral, penalize a miss less than a turn-down, occasional pings for low-ranked responders, an alert when someone's pings fall near zero; ping wait back toward 90s is a separate lever. Anything in config.py must be flagged to Marcus first, and Supply reads availability so load shifts can touch its scheduling.

- Module 5 (brief and prototype for Helen's request, 8 Oct 2026): Helen asked for a one-pager on what we'd build instead of quietly changing a number, seen from the person it happens to (a handler like Kip, a quiet responder like Meteor Mite), plus something clickable. Saved as 05-super-speed/brief.md (story form: Summary, Problem, Solution, How we'll know, Risks and questions) and 05-super-speed/prototype.html (handler console and Meteor Mite's iPhone as two separate screens). All numbers, dates and tags in the prototype are illustrative.
- The proposal, "quiet-responder recovery": handler can mark misses as not real (restores rank), an "Ease back in" ramp that resets to neutral and offers a few pings at a time (any answer, even a turn-down, counts; K silent offers in a row pauses and asks "still away?"), and plain-language status for handlers and responders. Does not revert 4.2, change proximity weights, change ping wait, ping anyone marked away, or identify anyone. N=3 probe pings, K=3 and the four ramp steps are my placeholders, not decisions.
- Code fact: availability.available_for() only returns responders marked available, so vacation or away responders are never ranked or pinged. The real risk is someone marked available but unreachable (Availability Confidence, squeezed out of 4.2) and the cost of a probe ping to them is a full ping wait. Misses vs turn-downs must be logged separately before any of this can be measured.
- Still open: Helen has not decided whether "those misses weren't real" is in version one; need Wen Li on decay speed, probe-ping placement and urgency, and whether scores reset on the 12 Aug deploy; Marcus on the unanswered 14 Aug question; Ravi on where the four sit (area vs callouts), the midnight-6am drop, and time-to-accept (not in the data today); Supply owner on scheduling impact. Priya's "seasonal" read is still untested against September.

- Module 6 (review-checklist skill, 9 Oct 2026): saved as a project skill at .claude/skills/review-checklist/SKILL.md (works only when this folder is open; a personal or account copy would make it global). Seven checks (owner, users, problem described and quantified, problem before fix, success metric, scope start matches end, timeline), each Pass/Partial/Fail; overall Ready only if all pass, Not ready on any Fail or more than three Partials.
- Reviewed 05-super-speed/brief.md: Not ready. No owner named (it is "for Helen Achebe", not by an owner), no timeline, success targets are only "toward" the old baselines with no dates, and the Summary lists "those misses weren't real" as part of the proposal while the last line asks Helen whether it is in version one. Users and the quantified problem pass. Fixes not yet applied to the brief. Laurie's version of the brief, in another GitHub repo, also failed on timeline (no dates) and was partial on scope and success metric.
- Scheduled task "monday-brief-review" (cron 0 9 * * 1, about 9:13 AM Monday, first run 12 Oct 2026) runs review-checklist read-only on every brief.md in this folder and reports back. It only matches files named brief.md, so the .txt files in 06-sidekicks/briefs are not covered. It is stored at ~/.claude/scheduled-tasks/, outside this folder; I created it on the student's explicit request after flagging the folder rule. It runs only while the Claude app is open. Still unknown: where the app's "Scheduled" menu is (student could not find it), and whether to click "Run now" once to pre-approve its tools.
