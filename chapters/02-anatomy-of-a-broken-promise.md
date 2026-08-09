# Chapter 2 — The anatomy of a broken promise
*Draft v1 (A, 2026-08-05 morning slice). Every number traces to FACTS.md (F1–F34).
Audit-LOCKED (PASS) 2026-08-08: every numeric literal cross-checked against its ledger
row by hand; one provenance gap found and fixed — "zero of 14,457" had no dedicated
ledger row, backfilled as F53 with independent 08-08 recomputation (same figure, two
routes). No forbidden-claims violations. No further changes needed to ship this
chapter as-is.*

My cross-validation promised 0.5462. The test delivered 0.6162. The natural way to
tell that story is as one gap with one villain — the model, the data, the luck. The
honest way turned out to be two separate stories: first the ruler was wrong, and
then, once the ruler was repaired, the world was different from the one the
training data described. Confusing those two is how a competitor learns nothing
from losing; separating them is most of what this write-up has to offer.

The ruler first. The promise itself — 0.5462 — was measured with an instrument that
flattered me twice. Rows whose session already appeared in the training fold were
warm: the model had seen that student on that day, and scoring those rows measures
memory, not prediction — worth +0.00432 of the flattery. And the cross-fitted
target encoding carried a fold-ID channel — an information-free column that a
grouped CV converts into signal — worth another +0.00230. Strip both, score only
cold rows with the leak removed, and the same model's honest promise reads
0.594. Before a gap can be decomposed, the instrument that measured it has to
be repaired; there is no point performing an autopsy with the scalpel that caused
the wound.

What remains between the honest promise and the test is +0.02244, and here the
write-up owes the reader something better than an estimate: the decomposition is an algebraic
identity on measured log losses, asserted in code to one part in a billion. It has
three positive terms. The largest, +0.01431 — 63.8% of the gap — is set
difficulty: the hidden test is intrinsically harder, its positive rate 0.685
against training's 0.7025, and no modelling decision of mine put it there. The
second, +0.00951, is the term my own first analysis nearly murdered. A specialist
pass flagged it as pooling artefact — warm and cold rows have different base
rates, so scoring them against one constant manufactures apparent skill — and
recommended writing it off. An adversarial re-test acquitted it instead:
a session with a single response can never be warm — zero of 14,457 such rows are
— so the warm/cold split is a relabelling of an observable covariate, responses
per session, and the identical statistic computed on the observable split
reproduces 101.1% of the term. Session length is a property of how tutoring
happens, not of my fold construction. That +0.00951 is real, transferable skill,
and it survived because the accusation was tested rather than believed. The third
term, +0.00662 — 29.5% — is the contamination itself, the two flattery channels
above, now priced.

The identity closes with two negative rows that keep everyone honest: −0.00114 of
honest cold-row skill the proxy still carried, and −0.00687 for the skill the
model actually delivered on the hidden test. That last number deserves to be
stated plainly, because it is the one the leaderboard never shows: against the
test's own entropy floor, my model transfers +0.00687 of real skill. The leader
transfers +0.02667 — 3.9 times mine. Both figures are single observations solved
from leaderboard scores measured once, with no error bar, and I will not pretend
otherwise. One bookkeeping caveat belongs in the open: the 0.6162 anchor is a
single GBM while the 0.594 proxy belongs to the ensemble built on it; paired
like-for-like the top-line gap is +0.01759, and every conclusion above survives
the substitution.

So the broken promise dissects into: a majority share that was never my error,
nearly a third that was my instrument flattering me, a real-skill term that my own
analysis nearly threw away, and a residue that says the model, honestly measured,
does transfer — at roughly a quarter of the best entrant's rate. Nothing in that
sentence was visible from inside the training data. Chapter 3 is about why it
could not have been: validation that lives where the training data lives cannot
see a distribution shift, no matter how carefully the folds are drawn.
