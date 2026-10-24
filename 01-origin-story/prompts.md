# 01 · Origin Story — prompts

**Context:** Rook Industries makes software for superheroes and the
people who handle them.

There are two products. Rook Dispatch is the one that gets a
superhero to where they're needed when there's an emergency — it
works out who is close enough and free enough to help, and gets hold
of them. Rook Supply keeps a responder's equipment serviceable and
accounted for, so a handler is never guessing whether the gear will
hold.

You joined two weeks ago as PM on Rook Dispatch. Release 4.2 shipped
on 12 August, shortly before you arrived. Responders have stopped
answering their phones the way they used to, and complaints have
gone up sharply. You were not in the room for any of it.

At the end of the session, type wrap up, and Claude Code saves the
prompts you wrote yourself below — not the starter prompt — updates
CLAUDE.md, and saves your work to GitHub. By Module 6 this file is a
prompt library built from your own questions.

---

### 1.

I'm hearing that "Release 4.2 shipped just before you arrived, and it landed badly" what has been key highlights of 4.2 and what's the riskiest here? What else have you noticed on metrics and user comments?

### 2.

What's contradictory or missing, not just in the company documents, but across Rook's wiki, its database, and the files in my repo?

### 3.

> How the four fell. The 60-second wait made the four miss more. They dropped below 60%, sank to the bottom of the list, and were then too rarely asked to earn their way back. That fits the "my phone never goes off" tickets, and it's close to the case Marcus asked about on 14 Aug.

This sounds like a strong problem hypothesis to explore deeper for sure. I wonder if the solution is rolling back or releasing an experiment to test a different design or logic


I would also suggest an experiment on the filter issue as that feels critical to the experience.

* Ping timeout tuning shipped as a flat cut. The roadmap says "tune" the ping wait; what shipped was a straight cut from 90 to 60 seconds. Availability Confidence appears nowhere in the code or the changelog. — Do we have any reference of experiment that they ran for this change?

1. No way to measure time-to-accept or coverage gaps. The database has no answer time, so the second headline metric can't be calculated. It also has no capability tags, so coverage gaps can't be measured — Big miss in my opinion, we must ensure we have an epic ready to refine the tracking and data mapping to be able to answer all the right performance data, without overdoing it of course
2. No Ravi. His weekly acceptance reports aren't in the wiki, and he isn't in the 4.2 thread. — Chase him
3. Nobody owns the roadmap. All six items are still assigned to Priya, and it was last reviewed on 30 June. — I own the roadmap, I'm the boss now

### 4.

* Write down how Dispatch decides who gets pinged (no doc exists), that's a great piece to delegate to Wen as I confirm the mechanism
