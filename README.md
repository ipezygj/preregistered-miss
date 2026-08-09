# The constant that caught my CV lying

*A measurement diary from inside a hidden-test competition: a preregistered
prediction, the miss that refuted it, and every number's receipt.*

I spent a summer competing on a hidden-test machine-learning task — predict, for
10,508 quiz responses, whether a K-12 student answers correctly — under a quota
of three scored submissions a week. I finished at rank 137. This is not a story
about winning. It is the complete, auditable record of what disciplined
measurement bought and failed to buy, including the part almost nobody
publishes: a frozen, hashed, dated prediction of my own model's hidden-test
score that turned out to be **wrong**, and the autopsy of which assumption broke.

The punchline table, preregistered on 2026-08-05 and scored on 2026-08-09:

| what was frozen | prediction (written before) | measured | verdict |
|---|---|---|---|
| ensemble, δ = 0 | 0.613, envelope [0.607, 0.619] | 0.6141 | inside — **hit** |
| 81-feature model | envelope [0.6168, 0.6216] | 0.6146 | outside — **miss** |

One hit, one miss, both published in the same table. That trade is the method.

## The chapters

1. **[The constant probe](chapters/01-the-constant-probe.md)** — the submission
   that predicted nothing and taught the most: a constant's log loss run
   backwards reveals the hidden test's own base rate, and with it, that my
   "failed" model was actually transferring positively.
2. **[Anatomy of a broken promise](chapters/02-anatomy-of-a-broken-promise.md)**
   — the gap between what cross-validation promised and what the test paid,
   decomposed as an algebraic identity: distribution shift, session
   composition, contamination, and the real skill that was almost thrown away.
3. **[What the folds cannot see](chapters/03-what-the-folds-cannot-see.md)** —
   why no within-training validation scheme, however honestly grouped, can see
   a distribution shift; ends by freezing the wager chapter 4 collects on.
4. **[The envelope and the miss](chapters/04-the-envelope-and-the-miss.md)** —
   the preregistered envelope, the score that landed outside it, and an autopsy
   that names which of two independent assumptions broke.
5. **[Five ways of asking whether effort shows from outside](chapters/04b-five-ways-of-asking.md)**
   — an interlude: five independent, increasingly expensive attempts to detect
   a student's reasoning process from the outside; five preregistered nulls.
6. **[What it recommends by example](chapters/05-what-it-recommends-by-example.md)**
   — the zoom-out: what a method whose showcase lost is still worth, and the
   four-sentence posture that transfers to any number produced by the party it
   flatters.

## How to audit this document

The chapters brag that "the checking has been made cheaper than the doubting."
Here is the checking:

- **[ledger/FACTS.md](ledger/FACTS.md)** — the fact ledger. Every numeric
  literal in every chapter traces to a numbered row (F1–F57) carrying its
  provenance: which script, which submission id, which date. The ledger also
  carries a *forbidden-claims list* — sentences the chapters are not allowed to
  contain because a ledger row refutes them — and each chapter's header records
  its audit status. (The ledger is kept in the project's working language,
  Finnish; the numbers, script names and submission ids are language-neutral.
  It is published verbatim rather than translated, because a retyped receipt
  is not a receipt.)
- **[ledger/PREREGISTERED.md](ledger/PREREGISTERED.md)** — the freeze record:
  SHA-256 per staged bundle, predictions written before each scoring window,
  the amendment that withdrew a plan when it collided with a rules ruling, and
  the results appended to the same tables afterwards. The original prediction
  text is never edited — history is appended, not rewritten.
- **[ledger/CORRECTIONS.md](ledger/CORRECTIONS.md)** — twenty-five numbered
  mistakes this project made, each priced in the currency it nearly cost, each
  ending in the concrete check that now prevents it.

The audits are real and they bite: they caught errors in these very chapters
before publication — invented numbers, unanchored claims — and those catches
are recorded in the chapter headers where they happened.

## Status and scope

The competition's model deadline is 2026-08-27; this record covers only my own
submissions, my own scores, and measurements derivable from them. No competition
data is included or redistributed. Chapter headers retain their working "draft /
audit-LOCKED" status lines deliberately — they are part of the provenance trail,
not editing debris.

## License

Text and ledgers: [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/).

---

*Ilpo Väätäinen — independent eval-integrity research: [github.com/ipezygj](https://github.com/ipezygj).
Related audits in the same spirit: [the 97% that predicts the past](https://github.com/ipezygj/dataco-late-delivery-audit)
(logistics ML's favourite benchmark grades itself), and [numguard](https://github.com/ipezygj/numguard)
(the Deflated Sharpe Ratio and Harvey–Liu FDR hurdle as runnable reference implementations).*
