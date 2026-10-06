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
