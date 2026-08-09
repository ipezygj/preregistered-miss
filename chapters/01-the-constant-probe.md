# The constant that caught my CV lying
*Chapter 1 — the base-rate probe. Draft v1 (A, 2026-08-05). Every number traces to
FACTS.md. Audit-LOCKED (PASS), re-verified 2026-08-08 alongside ch2/ch3: no gaps, no
forbidden-claims violations. Note the mixed-precision pattern is intentional and
ledger-tracked (F36): train rate cited at full precision (0.702469) while the test
rate stays at 3dp (0.685) throughout — keep that asymmetry when editing, don't
"fix" it by restating the test rate more precisely without re-checking F36.*

The submission that taught me the most predicted nothing. Working under a quota of
three scored submissions a week, I spent one of them on a constant — every test row
assigned the same probability, 0.7025, my training set's base rate. It came back
scoring 0.6238, and that one number quietly rewrote everything I thought I knew:
invert a constant's log loss and you get the hidden test set's own positive rate,
about 0.685, not the 0.702 my training data had promised. My model had scored
0.6162 — a result I had just finished misreading as a failure to transfer. Against
the training floor it looked broken; against the floor the probe revealed, it was
ahead. The cheapest diagnostic I ran caught my evaluation lying before it caught my
model failing.

Some context for how much that slot was worth. The task was to predict, for each of
10,508 quiz responses in a hidden test set, the probability that a K-12 student
answers correctly — a code-submission competition, so the model runs on the
organizers' machines against data nobody outside sees. Three scored submissions a
week; nine to twelve in the whole remaining competition. Every instinct says spend
them all on models. My cross-validation — grouped by session, honestly built, or so
I believed — was reporting 0.5462. The first real submission returned 0.6162, and
the natural reading of that gap is the one I reached for: the model doesn't
transfer. The natural reading was wrong, and I could not have found that out from
the training data at any price.

Here is what one constant buys. A constant prediction has a log loss that depends
on exactly two things: the value you submit and the base rate of the labels it is
scored against. Submit it and the equation runs backwards — the score hands you the
one number the organizers never publish, the test set's own positive rate. Mine
came back 0.685 against the 0.702469 my training labels showed. That difference is
not a rounding error; it is a distribution shift, confirmed by the organizers on
the competition forum: the test set is not drawn from the same source as the
training data. And it re-prices every score in the competition. The fair floor is
not the training floor but what a constant tuned to the test's own rate would
score — about 0.623. Even my mis-tuned constant, still carrying the training
rate, scored 0.6238; the model's 0.6162 beats both floors — it transfers
positively after all. Two floors, and the distance between them is the price of
the shift, drawn a second way. The probe didn't just measure the test set; it corrected the
story I was telling myself about my own result.

The general lesson costs nothing to state and a submission slot to believe:
validation that lives entirely inside your training data cannot see a distribution
shift, no matter how carefully you group your folds. The only instrument that can
is one whose answer you know in advance, scored where the shift lives. A constant
is the cheapest such instrument there is — one line of code, zero model risk, and
it converts a scored submission into a measurement device. Chapter 2 starts from
the gap this probe exposed — 0.5462 promised, 0.6162 delivered — and shows that
before the gap can be cut, the promise itself has to be repaired: a clean ruler
moves 0.5462 to 0.594, and only what survives that repair falls into three
measured parts.
