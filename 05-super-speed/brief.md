# Quiet-responder recovery

Product brief · Rook Dispatch · DRAFT for Helen Achebe · 8 Oct 2026
Based on Dispatch data from 29 Jun to 6 Sep 2026, 147 support tickets, four handler interviews and a read of the routing code. Anything I haven't confirmed is marked as such.

## Summary

Since release 4.2 on 12 August, a handful of responders have gone almost completely quiet. They are available, they haven't left, and Dispatch has simply stopped offering them work. Nothing in the product lets them back in, and nothing tells their handlers why. We propose to build a way back: a way for handlers to say "those misses weren't real", a gradual "ease back in" for responders who've been quiet for a while, and a plain explanation for both sides of what's going on. We are not reverting 4.2 and we are not just changing a number.

## The problem

Picture Kip, a handler who looks after two responders, Meteor Mite and The Gale. In the first week of September he's at his desk at midnight with both of their cards open side by side. The Gale hasn't stopped all week and is worn out. Meteor Mite's phone has been silent, and Mite has been texting Kip to ask whether something is broken or whether they've been forgotten. Kip has no answer. The console shows him two cards and no explanation, so he tells Mite to hang in there, because there's nothing else he can say.

Mite isn't alone. Four responders (Farlight, The Undertow, Vesper and Meteor Mite) were taking about 28% of all the pings that got answered before 4.2. Afterwards they took about 1%. Together they went from 49 pings a week to 8, and the number was still falling at the end of August. Before the release they were ordinary: answering roughly three pings in four and getting 11 to 15 offers a week. Meanwhile the other ten responders picked up around 25% more work. Handlers wrote in: 45 tickets say a phone never goes off, or that the offer was gone before they could answer. There were none of either kind before 12 August, and every one is still open. Two of the four, Mite and Vesper, never filed a ticket at all, so the tickets understate it.

The cost lands on the people Rook exists for. Callouts that ended with nobody taking them roughly doubled, from about 5.5% to 11%, and the average callout now needs more offers (1.39, up from 1.23) before somebody says yes. Part of that is a soft August, and we can't yet say how much of it is this problem. We also have no response-time data, so we can't tell you how much slower help arrived. What we can say is that more incidents are going unanswered, and a group of capable responders who used to be busy now sit unused.

**Why it happens.** Dispatch keeps a running score for each responder, based on how often they say yes. A yes raises it, and a "no" or a missed ping lowers it. Because the shorter ping wait in 4.2 (60 seconds, down from 90) made misses more likely, some responders slipped down the ranking. And once someone is ranked low enough they stop being offered work, which means they never get the chance to say yes and climb back. Nothing in the system ever lets the score recover on its own, and an engineer asked in 2019 whether it should and nobody decided. We're still checking whether location also plays a part, since the same release made proximity count for more.

## The solution

Here is the same evening for Kip after this ships. He opens Meteor Mite's card and sees that her pings are down 90% from her usual, along with the reason: her ranking dropped after several missed pings in mid-August. He now has something true to tell her. If those misses happened because Mite was dealing with an emergency, Kip marks that stretch as "those misses weren't real", and her ranking goes back to where it was. That handles the one-day blip, and she's back to full strength the next day.

If the gap was longer, say three weeks away, Kip clicks **Ease back in**. Dispatch starts her at a neutral ranking and offers her a small number of pings, placed where a miss costs the least. Any answer counts as a good sign, including "no", because it shows she's there and reachable. As she answers, the offers widen step by step until she's ranked like everyone else. If she stays silent, the offers stop and Dispatch asks Kip whether she's still away. On her phone, Mite sees what's happening in plain words instead of silence. Dispatch can also suggest the ease back in when it notices a responder's pings have dropped near zero.

For everyone else, little changes. The Gale keeps her place, and callouts are still offered first to whoever ranks highest. Only responders marked available are ever pinged, so nobody on holiday gets a ping, and no penalty builds up while they're away.

**What this does not do.** It doesn't revert 4.2, and it doesn't change how much distance counts. The proximity change was asked for and stays. It doesn't touch the ping wait either. That is a separate decision for Marcus and the team, and we'll raise it separately. It doesn't try to work out who anyone is: we use cover identity, capability tags, availability and callout history only, as Security Policy 4.1 requires. And it doesn't take on Kip's other requests, dark mode and different alert sounds for each responder. Those are real, but they're a different problem.

## How we'll know it's working

The clearest sign is Mite's phone. Responders who went quiet should return to roughly the number of offers they had before 12 August (the four's baseline was 49 a week between them). Alongside that, we'd expect acceptance to climb back from 68.5% toward the 76.8% we saw before the release, callouts nobody took to come down from 11% toward 5.5%, and missed pings to fall from about 110 a week toward the 25 we used to see. The 45 open tickets should close as handlers get an answer.

We'll also watch for harm. Offering work to someone who doesn't answer costs a full wait before the callout moves on, so we will watch how long it takes responders to say yes and how many callouts go unfilled, and stop the ramp if either gets worse. We can't measure time-to-accept today, so adding that is part of the work.

To improve it over time, every ease back in gets logged: how long it took, how many offers went unanswered, and how much delay it added. Ravi reviews the numbers each month and Wen adjusts the ramp, which ships in the monthly release. With only 16 responders there isn't enough data for the system to tune itself, so we're not promising that. Before launch, we'd replay June to September to see what ranking the four would have had under the new rules, then start with handler-triggered use only, so we control how many responders are affected.

## Risks and questions

**What if someone is on holiday?** They aren't pinged while marked unavailable, and they return from a break at neutral, with no old penalty attached.

**What if a responder is marked available but isn't reachable?** That is the gap Availability Confidence was meant to close, and it didn't ship in 4.2. Until it does, the handler and the "still away?" prompt are the safeguard.

**Could handlers abuse it to push their responders to the front?** We'll allow one ease back in at a time per responder, limit how often it can be used, and write each use to the audit log, as the 4.0 override log already does.

**Does shifting offers affect Supply?** Possibly. Supply reads Dispatch's availability record to schedule maintenance in quiet periods, so we'll tell the Supply team before shipping.

**What we still don't know.** We haven't confirmed what the four responders have in common, whether proximity is part of this, or whether the scores were reset when 4.2 deployed. Wen Li can answer how fast a missed ping should fade and how many offers the first step should include. Marcus's 14 August question, whether the change was meant to apply to responders already turning work down, never got an answer. It matters less now, since the four were ordinary before the release. And Priya's guess that this would sort itself out in September fits the call volume but not these four, though we have no September data to settle it.

**For Helen:** is the "those misses weren't real" action in scope for the first version, or should we start with the ease back in alone?
