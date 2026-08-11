# Chapter 4 — The envelope and the miss
*Draft v2 (2026-08-09). Shared opening drafted 08-08 before the score existed;
Branch B written 08-09 after [SCORE] landed as F54/F55 with provenance
(submission ids, date, platform score table; container logs not yet pulled and
said so on the ledger). Every number traces to FACTS.md (F35, F54, F55, F56).
Audit-LOCKED (PASS) 2026-08-09: every numeric literal cross-checked against its
ledger row; branch-A prose was never written, so there is nothing to have
backfilled. No forbidden-claims violations.*

## Opening (shared, no result-dependent claim)

Before the window opened we wrote down what a hit and a miss would each
mean, in the same document, side by side — because a prediction that can
still be reworded after the answer arrives is not a prediction, it is a
caption. The number below came from an 81-feature model trained on the
session anatomy this write-up has been building since chapter 2, frozen
against a hidden test set two independent ways, on 2026-08-05, four days
before the score that would confirm or refute it existed. The frozen
range was [0.6168, 0.6216], centred near 0.619. What follows is whichever
of the two branches drafted in ch4_outline.md turned out to be true —
written after the fact only in the sense that any report of a measurement
is written after the measurement, not in the sense that the target moved
to meet the shot.

## The result

The model scored 0.6146. The envelope said 0.6168 to 0.6216. It missed.

It missed on the side nobody advertises as a failure — the model did
*better* than the anatomy priced it, by 0.0022 against the envelope's
nearest edge — but the envelope was the claim, and the claim is false. A
prediction instrument that errs conservatively is still an instrument
that erred, and this chapter exists because chapter 3 ended with a
promise: if it lands outside, that failure gets published in the same
table as the successes. Here is the table.

| what was frozen | prediction (written before) | measured 2026-08-09 | verdict |
|---|---|---|---|
| ensemble, δ = 0 | 0.613, 95% envelope [0.607, 0.619] | 0.6141 | inside — hit |
| 81-feature model | envelope [0.6168, 0.6216], centre ~0.619 | 0.6146 | outside — miss |

One hit, one miss, both preregistered, both published. The hit is worth a
sentence of its own before the autopsy: the ensemble had never been
scored — every prior number belonged to its `complex` member, whose
recorded anchor was 0.6162 — and its prediction came from the same ruler
apparatus that produced the failed envelope. On the model family the
ruler was built from, it works. That is exactly the boundary the miss is
about to draw.

## The autopsy

The envelope came from two independent routes, and naming which one broke
matters more than the fact that something did.

Route A started from an anchor-ruler proxy of 0.599204 and added the
transfer gap observed within the model family, centred at 0.020015.
Route B started from a clean-cold proxy of 0.657552 and subtracted a
cold-row delta of −0.0401, measured on a sample of two models — a number
whose narrowness we flagged, at freeze time, as delta-agreement rather
than certainty. Against the measured 0.6146, route A's centre was off by
0.0046 and route B's by 0.0029. Both missed in the same direction: the
81-feature model carried more of its training-time skill onto the hidden
test than either route allowed. The clean-cold route — the one built on
rows deliberately stripped of the contamination chapter 2 quantified —
was the closer of the two, which is some comfort to the method and none
to the envelope.

What actually broke is narrow, and it is worth stating with the same
precision the freeze used. Chapter 2's decomposition is an algebraic
identity over already-measured numbers; it holds to 1e-9 and no future
submission can unmake it. What the miss falsifies is the step that turned
that identity into a forecast: the assumption that a gap measured inside
one family of models prices the transfer of a model from outside it. It
does not — or at least it did not here, and the platform's four-decimal
scoring leaves no room to argue the miss away as measurement noise. The anatomy remains a correct account of where the
old numbers came from. It has failed, once, cleanly, as an instrument for
new ones — out-of-family, where it had never been tested, which is why it
was tested there.

Two smaller facts belong in the record rather than a footnote. The
81-feature model landed 0.0005 behind the ensemble — everything it
carries beyond the shallow core bought, on the hidden test, five
ten-thousandths of log loss, in the wrong direction. And the window's byproduct settled a
standing question: the ensemble beats its own best member by 0.0021, so
the new best score on the board is 0.6141. Neither number rescues the
envelope. Both are what the window was priced to buy: information.

## What a published miss purchases

It would have been easy to make this chapter a hit. Widen the envelope
until it cannot lose; report the direction ("we predicted the big model
would be worse than the ensemble, and it was"); quietly promote route B
to "the" prediction after seeing it come closer. Every one of those moves
produces a cleaner-looking chapter and a less trustworthy one, and the
difference between them is invisible to any reader who was not shown the
freeze. That is the entire case for freezing: the miss is only
falsifiable because the envelope could not move, and the envelope could
not move because it was written down, hashed, and dated before the
platform could answer back.

A reader deciding whether to trust this write-up's method now has
something better than a string of successes: a calibration point. The
instrument prices in-family models correctly (the hit), overprices
out-of-family degradation (the miss, direction and both route errors on
file), and the next freeze — there will be one — inherits those error
bars instead of starting from faith. A suppressed miss would have cost
one awkward chapter and poisoned every confident sentence before and
after it. A published one is the receipt that the confident sentences
were earned. That trade is the whole method, and it is the last thing
this story recommends by argument. What it recommends by example is the
harder question — and before that, a narrower one: does the effort
behind a model's answer show up from outside at all?
