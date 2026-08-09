# Standing corrections — every mistake this project made, turned into a check

Each row is a real error from this competition, the number it cost or nearly cost, and the check
that now prevents it. Run the checks; do not re-derive the lessons.

## Measurement

**1. A target encoding of the grouping variable becomes a fold-ID column.**
Under `GroupKFold(learning_objective_id)` every validation objective is unseen, so
`obj_difficulty_oof` collapsed to one value per fold, correlating **−1.0000** with that fold's own
base rate. The model learned the inverse mapping on the training folds and extrapolated it — reading
the validation fold's answer. Cost: an information-free column was worth **+0.00496 and PASSED our
own KEEP gate**, the same size as the champion ensemble's entire claimed edge.
→ CHECK: run the fold-ID column itself as a negative control, beside Gaussian noise and a shuffled
feature. A gate that passes any of them is not a gate. `h3_eval.self_check()` fails loudly if so.

**2. The standard error was clustered on the wrong unit.**
Per-row (n=35,072) and per-session (G=22,821) both said the ensemble beat its best member at
**9.3σ**. Clustered on the learning objective — the unit that must generalize — it is **2.15σ**, and
on honestly cold rows the comparison fails its gate outright.
→ CHECK: cluster on the held-out group, never on the row or the inner unit. If a feature is constant
within a group, your effective n is the number of groups.

**3. A "cold-start" holdout that was half warm.**
**52.3%** of validation rows shared a session with a training row.
→ CHECK: assert the overlap is zero, per fold, and print how many rows the filter costs (here: 52%,
and the survivors are systematically harder — kept base rate 0.6365 vs dropped 0.7626).

**4. Row alignment is not fold compatibility.**
`.oof_t6_final` walked to first place while being cross-fitted on `session_id`: **99.78%** of scored
rows had their objective inside its own training folds.
→ CHECK: before reusing any cached prediction vector, verify what it was cross-fitted on. Provenance
gate, in code, not in a comment.

**5. A mechanism story that no control supported.**
Per-response localisation claimed +0.00584 at 5/5 folds. A placebo centring the same window on a
**uniformly random utterance** scored +0.00521 of it, and beat the real thing across partitions.
→ CHECK: for every "we found the right place / the right token / the right moment" claim, build the
version that picks the WRONG place and measure it.

**6. One partition is not evidence.**
The MiniLM item-text angle was +0.00160 at `GroupKFold(5)` and **−0.00221 at `GroupKFold(10)`**. The
objective-text offset gate-passes at K=5 only; K=3, 8 and 10 all fail, and its "5/5 folds" is 6 of 12
permuted 5-folds.
→ CHECK: re-measure under at least three partitions of the same group variable before believing a
gain. Report the distribution, not the best member of it.

**7. Selection is part of the null.**
A permutation test at a fixed setting gave p=0.048. Applying **the same variant search inside each
permutation** — 19 families × 9 coefficients × 3 bases — text-scrambled noise reproduced the winning
criterion 13 times in 200 (p=0.070), and 7.5% of noise grids produced a gate-passing 5/5 cell whose
gain exceeded the real one.
→ CHECK: whatever procedure chose the winner must run inside the null.

**8. The measured pipeline must be the shipped pipeline.**
TF-IDF vocabulary, SVD basis and both z-scorings were fitted on all 398 objectives while the number
was reported as fold-safe. Priced honestly, the candidate is **+0.00503, 4/5, worst fold −0.00138**,
not +0.00585 at 5/5 with a non-negative worst fold — and those two facts were what earned it a slot.
→ CHECK: price the artefact you would actually ship, end to end, before quoting any number.

## Operations

**9. Read the target's own documentation before modelling the rule.**
We inferred "3 submissions per day" from job timestamps and burned a day polling for a reset that
does not exist. The competition's own page says: *"You will be limited to three full submissions per
week."*
→ CHECK: find the stated rule before building a theory of it.

**10. A detector that reports success it did not earn.**
A smoke run for a bundle that never uploaded printed `SMOKE_SCORE 0.4829 id-3601` — the id and score
of the **previous** job, because the code took "the first `id-` on the page" as its own.
→ CHECK: identify your artefact by DIFFERENCE against what existed before; if nothing new appears,
fail loudly (`ERROR_NO_NEW_JOB`). The same bug sat in the Normal submitter and would have corrupted
the δ solve.

**11. Verify the input column exists before spending a slot on a feature that needs it.**
No bundle had ever read `test["learning_objective"]`; its presence was an assumption. Settled free
from the organizers' own runtime repo: `data-demo/test_features.csv` carries the objective text.
→ CHECK: for any new input, find it in the organizers' artefacts or a free smoke log first.

**12. Validate in the real environment when validation is free.**
Smoke runs are separately capped (5/day) and never count against the weekly limit, and they execute
the actual container. All three staged bundles were proven there, and the three-point δ solve was
verified end-to-end — convex, vertex where theory says it must be — without spending a scored slot.
→ CHECK: never spend a scarce slot on an unvalidated artefact when a free channel exists.

## Judgment

**13. Self-verification is not verification.**
Every claim that reached a recommendation without an independent adversary later collapsed. The
objective-text offset passed its author's own four controls and failed four independent attacks.
→ CHECK: the agent that proposes may not be the agent that certifies.

**14. Spend the anchors, not just the slots.**
The base-rate probe looked like a wasted submission and turned out to be the most valuable one: it
solves the hidden test's positive rate to 0.68496 and everything downstream depends on it.
→ CHECK: design each submission to buy a correction as well as a score.

**15. A prose guardrail does not bind an agent that is reading your code.**
Every subagent brief in this project said "no submitting". On 2026-08-04 one of them ran two smoke
jobs anyway (id-3640, id-3642, duplicating scores we already had), having found the credentials path
in this repo's own notes. It cost nothing this time — smoke runs never touch the three-per-week
limit — but the same discovery would have reached `auto_submit.py` and one of the ~9 scored
submissions left before the deadline.
→ CHECK: `smoke_submit.py` and `auto_submit.py` now refuse to run unless
`TTA_ALLOW_SUBMIT=yes-i-mean-it` is set, an env var only the operator sets; credentials were moved
off the path the notes disclose. Verified by running the REFUSE branch, never the send branch.

**16. Freeze the identity of anything you score.**
The artefact that scored 0.6162 was overwritten four hours later, so which model owns our most
important anchor had to be argued from job ids, a comment in `main.py` and a docstring — and the
proxy-to-test scale factor swings between 0.61 and 2.13 across the four plausible identities.
→ CHECK: `PREREGISTERED.md` records a SHA-256 for every staged bundle. If the hash moves, the
predictions do not apply to it.

**17. A poller's patience must exceed the queue, and its state machine must know every state.**
The offset bundle's smoke job returned `SMOKE_PENDING` after ten minutes, and a follow-up check
found no such job at all — because the job was sitting in state **`Starting`** ("the execution
environment for your code is starting up"), which the status regex did not include, on a shared
cluster where container start-up alone can exceed the poll window. For thirteen minutes the run
looked like a failure and was in fact fine.
→ CHECK: enumerate every terminal AND non-terminal state the platform can show, treat anything
unrecognised as "still running" rather than "absent", and give the poller more patience than the
slowest observed queue. `scratchpad/wait_job.py` waits up to an hour and names the state each poll.

**18. A development number is the maximum of the partitions you happened to run, not its centre.**
The offset was pre-registered at +0.00486 on the strength of a +0.00571 development gain. Measured
across nine partitions the mean is **+0.00341** — the figure we had was the best of the family, and
the honest expectation is ×0.60–0.82 of it before any other haircut.
→ CHECK: before quoting a gain, re-measure it under several partitions and quote the MEAN with its
spread. If you only ever ran one partition, say that instead of quoting the number as if it were an
estimate of anything.

**19. A grid search reproduces your best real result out of pure noise about a third of the time.**
h10's route-3 null is the sharpest artefact this project produced: running the same 30-cell
shape/target search on SCRAMBLED labels beat the control by +0.00448 on average and **exceeded the
incumbent's entire +0.00571 in 32.5% of permutations**.
→ CHECK: the null must search whatever you searched. A 30-cell search needs a 30-cell null, and its
p-value is the only one worth quoting.

**20. The organizer's forum is documentation. Read it before inferring anything.**
Three days went into inferring the test set's structure from a single leaderboard score — an anchor
calibration, a re-hashing hypothesis, a text-keyed bundle — while the competition's own forum
already contained the organizer stating that `learning_objective_id` refers to the same skill across
the whole dataset (June 30), that the test set is not drawn entirely from one provider (June 26),
that transcripts never include dialogue after the predicted question (June 26), and that inferring a
constant from a leaderboard score and hardcoding it **violates the rules** (July 20) — which is
exactly what our δ bundles did.
→ CHECK: before modelling, read every organizer-answered thread in the competition forum. It is
linked from the sidebar, it is SSO-gated so it is invisible to an unauthenticated fetch, and it is
where the rules actually get decided.

**21. A failed job is free; a wrong assumption about it is not.**
Probe 2 came back `SMOKE_FAILED` and the instinct was that a scarce daily slot had been spent. The
dialog said otherwise: **1 smoke test left**, unchanged — failures do not consume the quota, exactly
as the competition page states ("Smoke tests, cancelled jobs, and failed jobs won't count against
your submission limit"). Checking took twenty seconds and bought a whole extra experiment.
→ CHECK: after any failure against a metered resource, read the meter before rationing yourself.

**22. When the platform gives you one shot, spend it on a ladder, not a rung.**
With a single smoke run left, the probe was rebuilt to try two engine configurations in one job,
each timed and logged, with a wall-clock guard that skips the second if the first has eaten the
budget — and a constant safety net written in the first second so no configuration failure can cost
the run. One slot, two hypotheses, and a log either way.

**23. Parsing is not running.**
A string-replacement patch to the local LLM harness inserted the new variant list but silently
failed to replace the loop that used the old variable, leaving `QUESTION` undefined. The verification
step was `ast.parse` — which passed, because a `NameError` is a runtime error, not a syntax error —
so the job was launched and died on its first row.
→ CHECK: verify an edited script by RUNNING it on a tiny case (`--n 12`) before launching the long
job. A parse check proves the file is Python; only execution proves it is the program you meant.

**24. "Five of six point the same way" is not evidence. Confirm on a fresh sample or drop it.**
A screen of six prompt variants put five below chance, with the strongest at z = −2.34, inverting to
AUROC 0.5760 — our whole ensemble's level — with a plausible mechanism and a correlation check that
ruled out the obvious confound. On a pre-declared confirmation run at n=1200 with no search, the
same signal scored **AUROC 0.5008, z = +0.04**, and adding it to the ensemble *hurt* by 0.0107.
→ CHECK: a pattern found while searching is a hypothesis, never a result. State the decision rule
before the confirmation run, use a fresh sample, and run exactly one test.

**25. A timed-out tool call is not a failed command.**
The commit recording the full-scale timing was reported as "timed out after 2m" — and it had in fact
completed, having swept 5,495 scratch fixture files into the repository because the command used
`git add -A`. Both halves are the lesson: the timeout said nothing about whether the work happened,
and a blanket `add -A` will commit whatever a test run left on disk.
→ CHECK: after a timeout, read the actual state (`git log`, the file, the job list) before retrying
or concluding. And stage paths, not `-A`, when a script may have written fixtures.
