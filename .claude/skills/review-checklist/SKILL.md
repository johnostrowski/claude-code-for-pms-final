---
name: review-checklist
description: Reviews a product brief against a fixed checklist and flags what's missing. Use it when the student says "review-checklist", "run review-checklist", "review this brief" or "check this brief", pointing at a brief file in this folder or a brief in Rook's wiki. With no brief named (for example on its Monday schedule), it checks every brief under the wiki's "Product briefs" page plus 05-super-speed/brief.md.
---

# review-checklist

Check a brief against the eight checks below, the same way every time. Don't
improvise extra checks, and don't rewrite the brief. Report what's there and
what isn't.

## 1. Find the brief(s)

- **A file named by the student:** read it from this folder.
- **A brief named by title** (for example "Bulk Callout"): find it with the
  rook-wiki `search_wiki` tool and read it with `read_page`, including its
  comments.
- **Nothing named:** read every child page of the wiki's "Product briefs" page,
  plus `05-super-speed/brief.md`.

Only read. Never edit a brief, and never read or write outside this folder
except through the rook-wiki tools.

## 2. Run the eight checks

Each check is a **yes** or a **flag**. Every result needs evidence: quote the
line that passes it, or quote the line that shows it's missing. Never pass
something on a summary.

1. **Owner named:** a named person or team owns the work from here. A role
   with no name, "TBD", "nobody's picked it up yet", or a byline that only says
   who wrote it ("From: Dispatch PM") is a flag.
2. **Success measure:** it says how we'll know it worked, as something you
   could count: a metric, a target or a guardrail. Describing what gets built
   isn't a measure.
3. **Scope stays bounded:** the ask at the end is the same size as the ask at
   the start. Flag it if new features, teams or tools join partway through.
4. **Problem before fix:** the problem, and who has it, comes before the
   solution.
5. **Claims backed by evidence:** the main claim about the problem has a number
   or a quote, and says where it came from (tickets, database, an interview).
   Flag a problem that's only asserted.
6. **A specific person it's for:** it names at least one real user (a
   responder, handler or quartermaster) and what changes for them. "Users" or
   "handlers" in general is a flag.
7. **System health (loops, load, recovery):** it shows how the change ripples
   through the system, not just its direct effect:
   - **Loops:** if the change affects a result that later decides who gets
     chosen, it says what stops that loop running away.
   - **Load:** it says how work will spread across the pool, with a guardrail
     at both ends: nobody idle for 5+ days, and nobody far above their usual load.
   - **Recovery:** anyone who drifts out of balance has a way back.

   Flag it if the success measure only tracks an average, if a loop has no
   brake, or if there's a state you can enter but can't leave.
8. **Tested before, watched after:** every number that changes is listed with
   its old value, new value and the evidence behind it. The change is replayed
   against past data or rolled out in stages. There's a rollback trigger, and a
   named person watches the first week. Flag "tuning" with no test, or several
   risky changes bundled with no way to tell them apart.

## 3. Report

For each brief, use this layout. Pad the dots so the answers line up.

```
<brief name>
  owner named ................... yes (<who>)
  success measure ............... yes
  scope stays bounded ........... NO — flagged
  problem stated before fix ..... yes
  claims backed by evidence ..... yes
  specific person it's for ...... yes (<who>)
  system health ................. yes
  tested before, watched after .. NO — flagged
  -> <n> flag(s): <one plain sentence per flag, with the quote that shows it>
```

End with one line: `<n> briefs checked, <n> flagged, <n> flags in total.`

Then give the student a short plain-English summary: which brief needs the
most work, and the one fix that would help each flagged brief most.

## 4. Scheduled runs

When it runs on its own, with nobody in the room:

- Check every brief, as in step 1 under "Nothing named".
- Save the full report as `06-sidekicks/review-runs/<YYYY-MM-DD>.txt`, with a
  `Started:` line at the top. Don't overwrite `06-sidekicks/scheduled-run-output.txt`.
  It's the course's example.
- Finish with the summary in step 3, so the student sees it first.
