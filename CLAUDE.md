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
21 Aug 2026, no overlap). Sources: rook-wiki, rook-database,
`00-rook/code/dispatch-routing`, and `00-rook/company/notes/handoff-from-priya.docx`.

### Company and products
- Software for independent masked **responders** and the **handlers** and
  **quartermasters** who support them. 241 staff, mostly remote. Billed per active responder.
  Monthly releases, 4.x.
- **Hard rule:** never store or infer a responder's legal identity. Read
  Security Policy 4.1 (not in the wiki) before touching responder records.
- **Dispatch** (mine): ranks responders and pings them about callouts.
  Console (handlers) and phone app (responders). Routing is where the risk is.
- **Supply**: requisitions, maintenance, field failure reports. Reads the
  Responder Availability Record that Dispatch publishes.

### Vocabulary
- **Callout** = an incident needing a responder. **Ping** = one callout offered to
  one phone. Outcomes: **taken**, **turned down**, **missed** (no answer within
  the **ping wait**, now 60s, the same for everyone).
- **Acceptance rate** = share of pings taken (headline metric, reported weekly
  in aggregate). **Time-to-accept** = median seconds to take.
  **Coverage gap** = no responder with the needed capability tags.
- **Routing priority**: rank by proximity (travel time), recent acceptance
  score, capability match. **Mutual aid / shared cover**: not supported; Q4.

### People (wiki comments use roles)
| Role | Name | Notes |
| --- | --- | --- |
| Director of Product | Helen Achebe | My director. Owns roadmap. |
| Engineering manager | Marcus Oyelaran | Runs Dispatch eng. Start here. |
| Staff engineer | Wen Li | Built routing. No written spec; talk to her. |
| Product designer | Sofia Marino | Console, phone app, Sept interviews. |
| Support lead | Nadia Hoffmann | Tickets. Hears handler pain first. |
| Data analyst | Ravi Menon | Weekly acceptance report (not in wiki). |

### Releases
- **4.0** (7 Apr): console navigation, profile redesign, routing override audit log.
- **4.1** (16 Jun): travel-time proximity, bulk callout, push reliability.
- **4.2** (12 Aug): proximity weight 0.45→0.60, recent-acceptance weight
  0.40→0.25, ping wait 90s→60s, filter persistence, three defect fixes.

### 4.2 findings (rook-database, 29 Jun–6 Sep; interviews 2–5 Sep)
- **Pings taken: 76.6% → 64.0%. Missed: 2.3% → 18.0%.** Callouts nobody took:
  5.5% → 11.1%. Tickets: 5–8 a week → 20–32 a week.
- **Four responders went almost silent:** Vesper, Farlight, Meteor Mite, The
  Undertow. Each went from 11–14 pings a week to about 1 by the week of 31 Aug.
  They missed about a third of their pings in the release week. Before 4.2,
  nothing in the data marked them out.
- **Likely mechanism (my reading of the code; Wen should confirm):**
  - Missed counts the same as turned down. Both cost 0.12 and a yes earns 0.08,
    so anyone taking under 60% of pings slides toward zero.
  - Nothing lets the score recover over time (Wen's 2019 TODO in `history.py`).
  - So the 60s wait caused misses, the misses sank their scores, and responders
    at the bottom of the list are rarely asked, so they can't earn their way back.
- **The interviews match:** Dot (Vesper) sees both pings vanishing and quiet
  weeks. Kip says Meteor Mite is quiet while The Gale, same area, is busier
  (about 11 → 4 pings a week vs 13 → 19). Ambrose: slower answers no longer
  land the job.
- **"Seasonal" doesn't hold:** the first week of August was normal (77.9%
  taken). The drop starts in the release week. There's no earlier year of data.
- **The aggregate hides it:** it recovered to 72.7% by 31 Aug, partly because
  the four quiet responders were barely pinged any more.

### Key contradictions and gaps
- **Missed vs. turned down:** recorded separately in the Glossary and the database,
  but scored the same in the code. Marcus asked on 14 Aug if that was intended.
  Wen never answered.
- **Release notes vs. the code:** the notes say "proximity weighted up." They
  leave out that the acceptance weight was cut.
- **The code README** says "everyone available is on the list." True, but the
  bottom of the list is rarely asked.
- **Tickets vs. interviews:** Vesper and Meteor Mite have zero tickets since 4.2.
  Their handlers raised it only in interviews. Neither source shows all four.
- **Priya** calls filter persistence cosmetic noise. Ambrose and several tickets
  report filters resetting or saving the wrong view.
- **Priya** says Marcus pulls numbers. The directory says Ravi owns them.
- **Ping timeout tuning** (roadmap: "tune") shipped as a flat cut from 90s to 60s.
  No experiment or test is recorded anywhere (wiki, code, handover).
  **Availability Confidence**, committed for 4.2, is in no release, changelog or code.
- **Requisition chains brief** cites Halloran wanting a second approval. In his
  interview, his complaint is slowness (the priority field does nothing). The
  brief's scope also grows mid-document.
- **Stale briefs:** Bulk Callout says unstaffed but shipped in 4.1. Audit Log
  shipped in 4.0.
- **Missing:** answer time (so no time-to-accept), capability tags (so no coverage
  gap), score history, override records, Ravi's reports, Security Policy 4.1.
  An unexplained drop in callouts in the release week (about 138 → 110).
- **Roadmap** last reviewed 30 Jun. The wiki still names Priya on all six items.
  **I own the Dispatch roadmap now.**

### Open actions (mine)
- **Routing:** confirm the mechanism with Wen, then decide between a rollback and
  an experiment that tests different scoring logic (e.g. missed ≠ turned down,
  score recovery, ping wait).
- **Filters:** run an experiment on filter persistence. Resets and wrong views hurt trust.
- **Tracking epic:** capture answer time, capability tags, score history and
  overrides. Enough to measure acceptance, time-to-accept and coverage gap per
  responder, without over-building.
- **Chase Ravi** for the weekly reports and per-responder numbers.
- **Roadmap:** re-own all items, and agree with Helen which deferred 4.2 items stay in Q3.
- **Delegated to Wen:** write down how Dispatch decides who gets pinged (no doc
  exists). She does it alongside confirming the mechanism with me.

Module 1 notes: every wiki page has now been read. The person pages hold nothing
beyond the Team directory. The richest evidence was the `pings` table, week by
week per responder, plus `config.py`, `history.py` and `offer.py`. Watch Halfmoon
and Corporal Ashgrove: their pings are sliding more slowly (about 11 → 8 a week).
Still open: why those four missed more in the release week (the interviews hint
at distance from the phone). The Module 04–06 files in the repo state course
conclusions ahead of time. Treat them as course material, not Rook evidence.
