# Brief: getting quiet responders back on the list

**For:** Helen Achebe · **From:** Dispatch PM · **31 Oct 2026** · **Built around:** Meteor Mite (Kip's responder, Eastgate)

## The problem, for one person
Before 4.2, Mite got about 12 pings a week. By the end of August it was 1. In that same week The Gale, Kip's other responder in the same area, got 15–20. Kip has two cards side by side on the console and can't explain the gap. Kip's words: *"Mite thinks Mite's been forgotten. Gale's exhausted."*

Mite wasn't turning work down. After 4.2 cut the ping wait to 60 seconds, Mite **missed** pings. Today's rules count a miss exactly like a no (−0.12). Only a yes adds points back (+0.08). Mite's score fell from 1.00 on 11 Aug to 0.04 on 29 Aug. Responders near the bottom are almost never asked, so Mite had no way back. The same happened to Vesper, Farlight and The Undertow. Together, the four went from 51 pings a week to 6.

## What we'd build (keeping the 60-second wait)
1. **A miss is not a no.** We still record missed pings, but they stop lowering the score. In the replay, this one rule keeps all four above 0.76.
2. **What Mite would feel: a "still available?" check-in.** After 3 missed pings in a row, Mite's phone asks *"Missed a few. Still available tonight?"* It also asks when Kip's card shows the 5-day warning (*"It's been quiet. Still available?"*), so someone who is already too far down to be pinged gets asked too. A yes resets the score to neutral (0.50), not to the middle of the line. That's simpler and harder to game. The trade-off: on launch day the others are near 0.94, so Mite still comes second on callouts at an equal distance and climbs back one yes at a time. A "not tonight" marks Mite away for the shift, so nothing is counted against Mite. This answers Wen's 2019 note: someone who has stopped taking work stays down, and someone who says "I'm here" comes back.
3. **What Kip would notice: a reason on each card.** Each coverage card shows *"Pings this week: 1 (usual: 12)"*. A flag appears when a responder goes 5 days without a ping: *"Mite hasn't been pinged since Tuesday. 4 missed pings lowered Mite's place in line."* Lopsided weeks then look explained rather than like a coincidence. Each card also shows a small line of the responder's score over the week, so Kip sees a slide as it starts, not after five quiet days.
4. **A quick no gets help there faster, so it earns a little.** If a responder turns a ping down within 40 seconds, the callout moves on at least 20 seconds sooner. Their score goes up slightly (+0.02), but only up to neutral (0.5). A no after 40 seconds costs −0.12, as today. The ping on the phone says *"Can't make it? Tell us now and help gets there faster."*

## What we're not doing
- No rollback of the 4.2 proximity change. Priya's point holds: it was asked for.
- No change to the 60-second wait yet. A 75-second test comes later, once we track answer time.
- Nothing that stores or infers a legal identity (Security Policy 4.1).

## How we'll know it worked
- **Mission metric:** share of callouts with a yes within X minutes.
- **Guardrail:** no available responder goes 5+ days without a ping.
- **Signals:** the quiet four back near 12 pings a week each, and "phone never goes off" tickets (30 so far) falling back toward zero.
- **Rollout:** by area or by week, not by splitting responders, because they compete for the same callouts.

## What's still missing before this can be built
- **Wen:** confirm how production stores scores. The repo keeps them only in memory, and we need score history for Kip's score line. Also explain why Vesper lost first place in Old Town on day one while her score was still high. This fix doesn't explain that.
- **Sofia:** design the check-in and the card reason so they read as help, not blame.
- **Decisions:** what X minutes is.
- **Ravi:** a weekly baseline per responder, so the guardrail can be measured.
- **Answer-time tracking:** we don't record how fast a ping is answered today, so the 40-second rule can't run until we do. Then check that late no's don't turn into misses.
