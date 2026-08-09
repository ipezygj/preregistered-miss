# Chapter 3 — What the folds cannot see
*Draft v1 (A, 2026-08-05 morning slice). Every number traces to FACTS.md.
Audit-LOCKED (PASS) 2026-08-08: every numeric literal cross-checked against its
ledger row by hand, no gaps found. Explicitly re-verified: F36's precision trigger
("seventeen thousandths") has NOT fired — the test-rate anchor is stated only at 3dp
("0.685") throughout ch1–ch3, never at F4's full 0.68496, so 0.702−0.685=0.017 stays
the correct wording. No forbidden-claims violations.*

Here is the uncomfortable claim this whole story has been building toward:
validation that lives entirely inside your training data cannot see a distribution
shift — not because you grouped your folds carelessly, but no matter how carefully
you draw them. My session-grouped cross-validation promised 0.5462. When I
repaired that instrument — removed the fold-ID channel, dropped the warm rows,
clustered the errors honestly — the repaired promise read 0.594. The test paid
0.6162. The repair was real work and it closed a real hole, and the largest single
piece of the remaining gap was still invisible to it: the test set is simply
drawn from somewhere else. The organizers confirmed as much on the competition
forum — the test is not sampled from the same source as the training data. No
rearrangement of training rows into folds can tell you that, because every fold,
however it is drawn, is still made of training rows.

It is worth being precise about what the folds *can* see, because the argument is
not that cross-validation is useless. Grouped folds saw the memorization risk and
priced it. The clean ruler saw the leak and the warm rows and priced those. What
none of them could price is the term that dominated the honest gap — the 63.8%
that chapter 2 attributed to set difficulty, the test's own positive rate of
0.685 sitting seventeen thousandths below the 0.702 my labels promised. That
number does not live in my data. An instrument built entirely from my data can
approach it only the way a map drawn in one country approaches the coastline of
another: by assumption.

What does see it costs one submission. The constant from chapter 1 is the
smallest possible out-of-distribution instrument — a measurement whose answer is
known in advance, scored where the shift lives — and one scored slot converted it
into the test's own base rate, its floor at 0.623, and a corrected reading of my
own model. The general form of the lesson is about budgeting, not modelling:
whatever your submission economy is — three a week, in my case — some of it
belongs to measurement rather than to hope. A probe that cannot win teaches; a
hopeful submission that loses teaches nothing except its own score.

There is one more step, and it is the one this write-up stakes its credibility
on. An anatomy that only explains the past is an autopsy; the test of chapter 2's
decomposition is whether it can price a model it has never seen. Before the next
submission window we froze exactly that wager: a prediction for an unscored
model's hidden-test score, derived from the decomposition through two independent
routes and written down before the platform could answer back — an envelope of
0.6168 to 0.6216, centre near 0.619, hashes and assumptions on file. If the
measured score lands inside the envelope, the anatomy
graduates from post-hoc account to working instrument. If it lands outside, that
failure gets published in the same table as the successes — which is, in the end,
the entire method this story has been recommending.
