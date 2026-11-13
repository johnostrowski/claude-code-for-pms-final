# Release 4.2: what broke, how I proved it, and the fix

Final project for Product School's "Claude Code for PMs" certification, cohort
ccpm-2026.1. Rook Industries is a fictional teaching scenario, not a real company.

## ▶ The final report

**View it live: https://johnostrowski.github.io/claude-code-for-pms-final/final-report.html**

Or download [`final-report.html`](final-report.html) and open it in any browser
(Chrome, Safari or Edge). It's one self-contained file: the comic, the charts and
the live prototype are all inside it. GitHub's own file view shows the code, not
the page, so use the live link or the download.

Presenting: ↑ ↓ to move between slides, **N** for speaker notes and a 5-minute
timer, **A** to jump to the appendix.

## The answer in four lines

- **What happened:** Release 4.2 cut the ping wait from 90 to 60 seconds. Missed
  pings rose from 2% to 23%, and the code scores a miss exactly like a "no", with
  only a "yes" earning points back. Four responders sank to zero and stopped being
  asked: **51 → 6 pings a week**.
- **How I proved it:** interviews against 147 tickets, every ping replayed from the
  database, the routing code read line by line, and what-if replays of the rules.
- **The fix:** keep the 60-second wait. Stop counting a miss as a no, give quiet
  responders a way back with a "still available?" check-in, and show handlers why
  work is lopsided.
- **The gate:** a review-checklist skill that checks every brief for system health
  and for being tested before and watched after. It runs on its own every Monday.

## My work, module by module

| Module | What I did | Where |
| --- | --- | --- |
| 1 · Onboard | Built a working context of Rook, the team and Release 4.2 | [`CLAUDE.md`](CLAUDE.md) |
| 2 · Listen | Compared 4 interviews with 147 tickets and the roadmap | [`02-super-hearing/prompts.md`](02-super-hearing/prompts.md) |
| 3 · Verify | Checked every ping against the tickets; found the number for Helen (51 → 6) | [`03-rewind/pings-used.csv`](03-rewind/pings-used.csv), [`comic`](03-rewind/meteor-mite-and-the-gale-comic.svg) |
| 4 · Inspect | Traced the root cause in the routing code and replayed the alternatives | [`04-x-ray-vision/verdict.md`](04-x-ray-vision/verdict.md) |
| 5 · Prototype | Wrote the brief for the Director and built a clickable prototype | [`05-super-speed/brief.md`](05-super-speed/brief.md), [`05-super-speed/prototype.html`](05-super-speed/prototype.html) |
| 6 · Automate | Built the review-checklist skill (8 checks) and scheduled it for Monday mornings | [`.claude/skills/review-checklist/SKILL.md`](.claude/skills/review-checklist/SKILL.md), [first scheduled run](06-sidekicks/review-runs/2026-11-11.txt) |
| Final | The presentation and its comic art | [`final-report.html`](final-report.html), [`final-report-art/`](final-report-art/) |

## My prompt library

The prompts I wrote myself, saved at the end of each module:
[Module 1](01-origin-story/prompts.md) ·
[Module 2](02-super-hearing/prompts.md) ·
[Module 3](03-rewind/prompts.md) ·
[Module 4](04-x-ray-vision/prompts.md) ·
[Module 5](05-super-speed/prompts.md) ·
[Module 6](06-sidekicks/prompts.md)

## Notes for the grader

- **00-rook/** is the course's source material (company wiki notes, the routing
  code and the handover note), left unchanged.
- The data comes from the course's rook-wiki and rook-database connectors.
  `03-rewind/pings-used.csv` is my checked copy of all 1,696 pings.
- The comic art in the final report was generated with Higgsfield, from one
  character sheet so the characters stay consistent. The originals are in
  `final-report-art/`.

## Course setup

1. Paste the course setup prompt (the one-line prompt in your pre-class
   materials) into Claude Code and follow along. Claude gets the setup steps
   from setup/SETUP.md in this course. Setup makes your own copy of this
   course on GitHub and puts it on your computer, in a folder called
   claude-code-for-pms-final. Your copy on GitHub is Public. Keep it that way
   until the course ends, so your instructor can see your work.
2. In the Claude app, start a new session on the claude-code-for-pms-final
   folder and type: check my setup
3. At the end of each module, type: wrap up

Each module folder has a `prompts.md` file. At the end of each session, wrap up
saves the prompts I wrote myself into it, updates CLAUDE.md, and saves my work
to GitHub.
