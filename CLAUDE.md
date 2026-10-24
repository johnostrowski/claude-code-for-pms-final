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
- Answer only the question the student asks, from the sources they
  name. Don't investigate beyond it or offer a theory about what
  Release 4.2 changed unless they ask for one.
- Read and write only within this directory.
  Exceptions, for the course-setup and wrap-up skills only:
  - When the student asks you to check their setup, save their work or wrap up a session, that request is their yes. You may run the GitHub command-line program installed at ~/.ccpm/gh for those checks and saves, and look in that folder to find it.
  - For a repair, first tell the student in one plain sentence what you are about to do, and act only after they say yes. Repairs may: run that GitHub program (including setting this folder's own git sign-in setting and changing this repo's visibility back to Public); copy the student's own course files into this directory from another folder on their computer (copy only; never move, edit or delete the originals); and rename something outside this directory that blocks setup, by adding "-old" to its name (never delete it).
  Outside this directory you still never write, edit or delete anything else.

<!-- Keep the block above at the top of this file. Everything you add
     during the course goes below this line. -->

---

## Working context

I'm the new PM for **Rook Dispatch**, taking over from Priya Raghunathan (left
21 Aug 2026, no overlap). **I own the Dispatch roadmap.** Sources: rook-wiki
(all pages read), rook-database, `00-rook/code/dispatch-routing`, Priya's
handover note. The Module 04–06 files state course conclusions ahead of time.
They're course material, not Rook evidence.

### Company and products
- Software for independent masked **responders** and the **handlers** and
  **quartermasters** who support them. Billed per active responder. Monthly 4.x releases.
- **Hard rule:** never store or infer a responder's legal identity (Security Policy 4.1, not in the wiki).
- **Dispatch** (mine): ranks responders and pings them. Console for handlers,
  phone app for responders. **Supply**: requisitions, maintenance, failure
  reports. It reads Dispatch's availability record.
- **Vocabulary:** callout = an incident. Ping = one callout offered to one phone:
  taken, turned down, or missed (no answer within the 60s **ping wait**).
  **Acceptance rate** = share taken (weekly, aggregate). **Coverage gap** = no
  responder had the needed tags. **Routing priority** ranks by proximity,
  recent-acceptance score and capability match.

### People (wiki comments use roles)
| Role | Name | Notes |
| --- | --- | --- |
| Director of Product | Helen Achebe | My director |
| Engineering manager | Marcus Oyelaran | Runs Dispatch eng. Start here |
| Staff engineer | Wen Li | Built routing. No written spec |
| Product designer | Sofia Marino | Console, phone app, Sept interviews |
| Support lead | Nadia Hoffmann | Tickets |
| Data analyst | Ravi Menon | Weekly acceptance report (not in wiki) |

### 4.2 (12 Aug): proximity weight 0.45→0.60, acceptance weight 0.40→0.25, ping wait 90s→60s, filter persistence
- **Pings taken 76.6% → 64.0%; missed 2.3% → 18.0%;** callouts nobody took
  5.5% → 11.1%. Tickets went from 5–8 to 20–32 a week.
- **Four responders went almost silent:** Vesper, Farlight, Meteor Mite and The
  Undertow. Each went from 11–14 pings a week to about 1. They missed a third of
  their pings in the release week. Halfmoon and Corporal Ashgrove are sliding more slowly (11 → 8).
- **Likely mechanism (my reading of the code; Wen to confirm):**
  - A miss costs the same as a turn-down (−0.12), while a yes earns +0.08, so
    anyone under 60% taken slides to zero.
  - Nothing lets the score recover (Wen's 2019 TODO).
  - So the 60s wait caused misses, scores sank, and the bottom of the list is rarely asked.
- **Score rebuilt from the pings** (Module 3, code rules; proximity and tags not
  in the database): the replay puts exactly the quiet four at 0, and everyone else
  at 0.72 or above. The fall is gradual, though. Mite went 1.00 (11 Aug) → 0.52
  (16 Aug) → 0.28 (22 Aug) → 0.04 (29 Aug). Vesper went 0.96 → 0.04 (26 Aug).
  The Gale and Captain Vantage dipped to about 0.6 and recovered.
  So the score explains why they can't climb back, not why their pings dropped first.
- **Open: Vesper in Old Town.** She was asked first on 19 of 21 Old Town callouts
  in the two weeks before 4.2, then on 0 of 6 in the release week, while her score
  was still 0.8–0.96. Captain Vantage, Nightwell and The Longcast were asked ahead
  of her. Old Town's volume held (about 11 → 10 a week). Not explained by the
  acceptance score. Likely the reweighting or proximity, which we can't see. Ask Wen.
- **"Seasonal" doesn't hold:** early August was normal (77.9%). The drop starts
  in the release week.
- **The aggregate hides it:** it was back up to 72.7% by 31 Aug, partly because
  the quiet four were barely pinged any more.
- **Interviews match:** Dot sees pings vanish and quiet weeks. Kip says Mite is
  quiet while The Gale is overworked. Ambrose: slow answers no longer land the job.

### Interviews (4) vs. tickets (147)
- **Different people:** Vesper, Meteor Mite, The Gale and Sgt. Bulwark have
  zero tickets. Only Ambrose is in both piles.
- **Interviews only:** handlers want an alert when a callout comes in (3 of 4
  interviews vs 2 tickets), and lopsided workload.
- **Tickets only:** "is my account broken?" (30), filter problems beyond
  resets, console bugs and records issues (36), Supply logistics, billing.
- Halloran says maintenance scheduling has "gotten smarter." 4 tickets disagree.
- Tickets are often near-identical templates, so the counts overstate how many distinct problems there are.

### Contradictions and gaps
- Missed and turned down are recorded separately but scored the same. Marcus
  asked on 14 Aug whether that was intended. No answer.
- The release notes omit the cut to the acceptance weight. The code README says
  "everyone available is on the list," but the bottom of the list is rarely asked.
- Ping timeout "tuning" shipped as a flat cut with no recorded experiment.
  Availability Confidence (committed for 4.2) never shipped.
- Priya calls filter tickets noise, but they report real resets and wrong views.
  Priya says Marcus pulls numbers, but the directory says Ravi owns them.
- The requisition-chains brief adds an approval step, but Halloran's complaint is
  slowness. Bulk Callout and Audit Log briefs are stale (both shipped).
- **Missing data:** answer time (so no time-to-accept), capability tags (so no
  coverage gap), score history, overrides. Also missing: Ravi's reports. The
  roadmap was last reviewed 30 Jun and still lists Priya on every item.
- Unexplained: why the four missed more, and the drop in callouts in the release week (138 → 110).

### My priorities
1. **Priority:** the two ping problems, "gone before I could answer" (16
   tickets) and "nothing again this week" (30 tickets). Confirm the mechanism
   with Wen, then choose between a rollback and an experiment on the scoring
   (miss ≠ turn-down, score recovery, ping wait).
2. **Quick fixes:** the feed jumping to the top (2), signed out too often (2), a
   warning when filters reset (4). Shared and cross-computer filter problems are bigger.
3. **End-of-season "one more thing":** dark mode (4 tickets + Kip).

### Open actions
- **Delegated to Wen:** write down how Dispatch decides who gets pinged, and
  explain why Vesper lost first place in Old Town on day one of 4.2.
- **Tracking epic:** capture answer time, tags, score history and overrides.
  Enough for per-responder acceptance, time-to-accept and coverage gap. No more.
- Chase Ravi. Re-own the roadmap, and agree with Helen which deferred 4.2 items stay in Q3.

Module 2 notes:
- **Interviews:** losing the race (3 of 4) and wanting a handler alert (3 of 4)
  were the top complaints, then quiet weeks (2) and small text (2).
- **Ticket groups:** quiet 30, Supply 26, console layout 25, gone-before-answer 16,
  filters 16, records 11, readability 8, notifications 7, sign-in 6, billing 2.
- **Roadmap fit:** only 3 of the 8 interview complaint groups touch the roadmap.
  Only the handler phone app helps. Ping timeout tuning made "losing the race" worse.
- **Open:** why Dot, Kip and Halloran never file tickets, and how many handlers like them we're not hearing from.

Module 3 notes:
- **Number for Helen: 51 → 6.** Pings to the quiet four in the week before 4.2
  vs the last full week. Overall % taken is back to 75%, so the average hides it.
- **Tickets vs pings agree:** every count or date a ticket quotes matches the
  pings. Misses start on 12 Aug itself. Silence follows about 5 days later
  (first "never goes off" ticket on 17 Aug).
- **Meteor Mite vs The Gale** (both Kip's, Eastgate): Mite was asked first on
  46% → 15% of their shared callouts, and went from about 12 pings a week to 1.
  The Gale went to 15–20.
- **Files:** `03-rewind/pings-used.csv` has all 1,696 pings (checked against
  the database). `03-rewind/meteor-mite-and-the-gale-comic.svg` is the
  story as a comic for presenting.
