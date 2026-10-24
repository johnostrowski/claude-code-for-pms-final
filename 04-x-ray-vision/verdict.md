# Module 4 verdict: why some responders stopped getting pinged

## Verdict

Release 4.2 cut how long a ping waits from 90 to 60 seconds. More pings went
unanswered, and the code counts an unanswered ping exactly like a "no." A few
responders missed a burst of pings in the first week and sank down the list.
The only way to earn points back is to say yes, and someone at the bottom of
the list is almost never asked. So they stayed quiet. 4.2 didn't create the
trap. It started sending people into it.

## Where it's written

All files are in `00-rook/code/dispatch-routing/`.

- `history.py`
  - line 35: a missed ping and a "no" both cost 0.12.
  - line 25: a yes is the only thing that earns points, +0.08.
  - line 30: Wen's 2019 note asking whether scores should recover over time,
    left unanswered.
- `offer.py`, lines 28–30: where each answer triggers the score change.
- `config.py`: the 60-second wait (90 before 4.2) and the plus and minus amounts.

## The evidence behind it

- **Misses:** went from 2% to 23% of pings in the release week.
- **The rebuilt score:** replaying it from the pings puts exactly the four quiet
  responders (Vesper, Farlight, Meteor Mite, The Undertow) at 0, and everyone
  else at 0.72 or above.
- **The what-if replay:** if a miss didn't count as a "no," none of the four
  would have dropped below 0.76.

## Where my findings add to the slide's verdict

- **The slide says the shorter wait "did this."** The replay shows the bigger
  lever is a miss counting as a "no." The shorter wait caused the misses, but
  take away that one rule and the four never sink. That's why the plan fixes the
  miss rule first and tests 75 seconds second.
- **One case isn't explained by this mechanism.** Vesper lost first place in her
  own area on release day, while her score was still high. That points at the new
  weighting or the travel-time data, which we can't see. Still open for Wen.
