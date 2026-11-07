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
21 Aug 2026, no overlap). **I own the Dispatch roadmap.** Sources: rook-wiki,
rook-database, `00-rook/code/dispatch-routing`, Priya's handover
(`00-rook/company/notes/handoff-from-priya.docx`). Files in the module folders
that state course conclusions are course material, not Rook evidence.

### Rook basics
- Software for independent masked **responders** and the **handlers** and
  **quartermasters** who support them. Billed per active responder. Monthly 4.x releases.
- **Hard rule:** never store or infer a responder's legal identity (Security Policy 4.1).
- **Dispatch** (mine) ranks responders and pings their phones. Handlers use the
  console. **Supply** reads Dispatch's availability record.
- **Ping:** one callout offered to one phone. It's taken, turned down, or missed
  (no answer within the 60s **ping wait**).
- **Routing priority:** travel time × 0.60 + recent-acceptance score × 0.25 +
  capability × 0.15. A yes adds 0.08. A no or a miss takes off 0.12. Scores run
  from 0 to 1 and start at 0.5.

### People (wiki comments use roles)
| Role | Name | Notes |
| --- | --- | --- |
| Director of Product | Helen Achebe | My director |
| Engineering manager | Marcus Oyelaran | Runs Dispatch eng. Start here |
| Staff engineer | Wen Li | Built routing. No written spec |
| Product designer | Sofia Marino | Console, phone app, Sept interviews |
| Support lead | Nadia Hoffmann | Tickets |
| Data analyst | Ravi Menon | Weekly acceptance report (not in wiki) |

### What 4.2 broke (shipped 12 Aug)
- **What changed:**
  - Ping wait cut from 90s to 60s.
  - Proximity weight up from 0.45 to 0.60, and acceptance weight down from 0.40
    to 0.25. The release notes leave out the acceptance cut.
  - Filter persistence.
- **The numbers:**
  - Pings taken fell from 76.6% to 64.0%. Missed rose from 2.3% to 18.0%.
  - Callouts nobody took rose from 5.5% to 11.1%.
  - Tickets went from 5–8 to 20–32 a week.
  - It isn't seasonal: early August was 77.9%. The average also hides it,
    because it was back to about 75% by the end of August.
- **Quiet four:** Vesper, Farlight, Meteor Mite and The Undertow. Each went from
  about 12 pings a week to 1. **Number for Helen: 51 → 6** pings a week between them.
- **Mechanism** (`04-x-ray-vision/verdict.md`): the 60s wait caused misses, and a
  miss costs the same as a no. Only a yes earns points back (`history.py:25`),
  and nothing lets a score recover (Wen's 2019 TODO).
- **Score replay:**
  - It puts exactly the quiet four at 0, and everyone else at 0.72 or above.
  - Mite fell gradually: 1.00 on 11 Aug to 0.04 on 29 Aug.
- **What-if replays:** if a miss weren't a no, the four end at 0.76–0.84. That's
  the main lever. Recovery alone is too slow (0.23–0.35).
- **Travel time needed to win:** at a score of 0, a quiet responder must be about
  18 min closer than a rival at 0.94. At 0.50, about 8 min.
- **Marcus's 14 Aug question, answered:** 4.2 applied to everyone the same.
  Misses after 4.2 predict who went quiet. Past turn-downs don't.

### What the sources say
- **Tickets vs pings:** they agree on every count and date. Misses start on
  12 Aug. The first "never goes off" ticket came on 17 Aug.
- **Interviews (4) vs tickets (147):** they cover different people. Vesper, Mite,
  The Gale and Sgt. Bulwark have no tickets.
  - Ticket groups: quiet 30, Supply 26, console layout 25, gone-before-answer 16,
    filters 16, records 11, other 23. Many tickets are near-identical templates.
  - Top interview complaints: losing the race (3 of 4) and wanting a handler
    alert (3 of 4).
  - Kip: Mite is quiet while The Gale is overworked.
  - Only 3 of the 8 interview complaint groups touch the roadmap.
- **Contradictions:**
  - Missed and turned down are recorded separately but scored the same.
  - The ping wait cut shipped with no experiment.
  - Availability Confidence, committed for 4.2, never shipped.
  - Priya calls the filter tickets noise. They aren't.
  - The roadmap was last reviewed on 30 Jun.
  - The Bulk Callout and Audit Log briefs are stale (both shipped).
  - The Requisition Approval Chains brief adds an approval step, but Halloran's
    complaint is slowness.
- **Missing data:** answer time, capability tags, score history, overrides and
  Ravi's reports.

### The fix (Module 5): `05-super-speed/brief.md` and `prototype.html`
- **The brief keeps the 60s wait and adds four things:**
  1. A miss isn't a no.
  2. A "still available?" check-in, after 3 misses in a row or 5 days with no ping.
  3. Kip's cards show usual pings, a reason, the 5-day warning and a score line.
  4. A quick no (within 40s) earns +0.02, up to 0.50.
- **Decided:** a yes to the check-in resets the score to neutral (0.50). It's
  simpler and harder to game, but Mite climbs back one yes at a time.
- **Rollout:** by area or by week, not by splitting responders. Then test a 75s
  wait, which needs answer-time tracking.
- **Mission metric:** share of callouts with a yes within X minutes (X not
  decided yet). Guardrail: nobody goes 5+ days without a ping.
- **Gap:** the brief still doesn't name an owner.

### Open
- **Wen:**
  - Write down how Dispatch decides who gets pinged.
  - Explain why Vesper lost first place in Old Town on day one, while her score
    was still 0.8–0.96.
  - Explain how production stores scores. The repo keeps them only in memory.
- **Tracking epic:** answer time, tags, score history and overrides. No more.
- **Chase Ravi.** Re-own the roadmap, and agree with Helen which deferred 4.2
  items stay in Q3.
- **Unexplained:** why the four missed more, and why callouts fell from 138 to
  110 in the release week.
- **After the ping fix:**
  - Quick fixes: the feed jumping to the top (2 tickets), being signed out too
    often (2), and a warning when filters reset (4).
  - Then dark mode (4 tickets, plus Kip).

### My files
- `03-rewind/pings-used.csv`: all 1,696 pings, checked against the database.
- `03-rewind/meteor-mite-and-the-gale-comic.svg`
- `04-x-ray-vision/verdict.md`
- `05-super-speed/brief.md` and `prototype.html`
- `.claude/skills/review-checklist/SKILL.md`

Module 6 notes:
- **review-checklist skill, 8 checks:** owner, success measure, scope, problem
  before fix, evidence, a specific person, **system health** (loops, load at both
  ends, recovery) and **tested before, watched after** (old→new values, replay
  or staged rollout, rollback trigger, a named person watching week one).
- **Why 7 and 8:** 4.2 failed on system effects, not on how it was written. My
  Module 5 brief passes checks 1–6 mostly but gets flagged on 7 (no ceiling for
  The Gale at 15–20) and 8 (no rollback trigger or watcher). Also flagged on owner
  and scope (item 4).
- **The 4.2 roadmap items** ("Change to who gets pinged", "Ping timeout tuning")
  are one line each, owned by Priya. They fail 6 of 8. The real gap: they had no
  real brief to review.
- **Ideas not built yet:** require an owner who's still in the Team directory,
  and flag committed roadmap items that have no brief.
- **Scheduled:** "Rook course: Monday brief review", every Monday at 08:12 in
  Auto mode. It saves reports to `06-sidekicks/review-runs/` and only runs while
  the app is open.
