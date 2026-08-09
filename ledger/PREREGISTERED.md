# Pre-registered: the 2026-08-09 window

Written **before** the window opens. Two rules from `CORRECTIONS.md` are enforced here: never ship a
bundle whose score has not been predicted in writing first, and freeze the identity of every scored
artefact — the model that scored 0.6162 was overwritten four hours after it scored, and a whole
section of `h6_anchor_calibration.md` had to argue from timestamps which artefact it had been.

## The artefacts (frozen 2026-08-04, SHA-256)

| slot | bundle | sha256 | container-validated |
|---|---|---|---|
| 1 | `submission_ensemble.zip` (δ = 0) | `288db0427667e6538929356a137bf6a0adeda42967110935c5b25367292be55a` | job id-3601, exit 0, 100 rows |
| 2 | `submission_ens_d1.zip` (δ = −0.08224) | `78a29d4db6af56a35e2e173238abceabf43376c2dcfd2156a00259c405246e40` | job id-3602, exit 0 |
| 3 | `submission_ens_d2.zip` (δ = −0.16448) | `e76def7a8849d76a8377ae6259e32a6d79be5e52dd26987c4f458d2c8cccdceb` | job id-3607, exit 0, 100 rows |

If a hash changes, the bundle is not the one described below and the predictions do not apply.

## The predictions

All three bundles are the same model at three prior levels, so the window measures the **level axis**
three times. Two candidate populations disagree, and they disagree in **sign** — the measurement
(paired differences on one fixed test set, exact to ±0.0001) is roughly twenty times finer than the
disagreement, so this resolves cleanly either way.

| quantity | cold-row shape (A) | all-row shape (B) |
|---|---|---|
| slot 2 − slot 1 | **+0.00036** | **−0.00100** |
| slot 3 − slot 1 | +0.00218 | −0.00059 |
| implied argmin δ\* | −0.0207 | −0.0996 |
| slot 1 absolute | ≈0.613, 95% [0.607, 0.619] | — |

**What each outcome means.** If slot 2 beats slot 1, the shipped model's mean prediction on the
hidden test sits above the optimum and the label-shift correction is real; if it loses, our mean
prediction was already below it and δ overshoots. Either way the three points fit a quadratic whose
vertex gives δ\* directly, which prices every remaining submission — including the objective-text
offset held back to 08-16.

**Ruler pre-registration.** These predictions come from `h6_slot_predict.py`. After the window the
scale fit will have five differences against one parameter — four residual degrees of freedom
instead of one — which is what finally makes "the clean ruler beats the contaminated one" a testable
claim rather than the 0.36 anchor-SE of noise it is today.

## What this window does NOT measure

Three levels of one model measure discrimination **zero times**. The 08-16 window's first slot
should therefore be a one-change model swap, not another level. One change per bundle, always.

## Open before shipping

* Confirm whether the reset is a rolling 7 days (≈14:47 UTC) or a Sunday calendar week (00:00 UTC) —
  check early on 08-09 rather than assuming; assuming the rule already cost one full day.
* Submitting is operator-only: `TTA_ALLOW_SUBMIT=yes-i-mean-it` must be set by hand. A subagent
  briefed "no submitting" ran two smoke jobs (id-3640, id-3642) on 08-04 by finding the credentials
  path in this repo's own notes.

---

# Pre-declaration: the objective-text difficulty offset, struct-only

Written **before** the ship-faithful re-measurement and the permutation null are run, per condition 3
of `h5_ruling.md` §8 ("the selection burden is only discharged if the choice is made once, in
advance, and recorded"). The git commit containing this section precedes the commit containing the
numbers; that ordering is the point.

**Declared coefficient: b = 0.20.** Mid-plateau, and the value the h5 adjudication already priced
ship-faithful at +0.00571, CI [+0.00102, +0.01053], 4/5 folds. It will not be re-optimised. If the
re-measurement disagrees with that figure, the re-measurement stands and b stays 0.20.

**Declared form:** struct only — no word/TF-IDF proxy, no SVD asset, no tokeniser reimplementation.
Centring constant frozen at c = 0.10269040663349374. Additive logit offset on the shipped ensemble,
applied to rows whose objective is not resolvable in the train table.

**Declared null:** 200 seeds, permuting the objective→difficulty assignment across held-out
objectives, run through the identical fold-safe pipeline at the same fixed b. The p it produces is
the pre-declared p, superseding the 20-seed transductive p = 0.048.

**Declared decision rule, so the result cannot be reinterpreted afterwards:** the offset takes a slot
in the 08-09 window if the objective-clustered CI excludes zero AND at least 4 of 5 folds improve AND
the permutation p is below 0.05. If it fails any of the three, it does not ship and the slot reverts
to δ = −0.16448.

**Condition 8 is already satisfied.** It required a smoke log proving `learning_objective` is present
and non-null in the inference environment before any bundle is built. Today's text-keyed smoke run
(job id-3724, exit 0) printed `objective resolved by text for 100/100 rows`, which cannot happen
unless the column is present, non-null and readable; the organizers' runtime demo carries the same
header. That pre-condition is discharged.

## Result of the pre-declared measurement (run AFTER the declaration above)

`h9_offset_final.py`, struct-only, b = 0.20, frozen c, fully fold-safe:

    LL 0.64728   gain +0.00571   CI [+0.00102, +0.01053]   4/5 folds   worst −0.00166
    per-fold  +0.00381  +0.00563  +0.00828  +0.01223  −0.00166
    AUROC 0.5796 -> 0.5990  (+0.0194)
    permutation null, 200 seeds, identical fold-safe pipeline, b fixed:
        mean −0.00099  sd 0.00260  max +0.00865   real +0.00571   z +2.58   p = 0.0100

All three pre-declared criteria pass (CI excludes zero, 4/5 folds, p < 0.05), so by the rule fixed in
advance the offset **ships in the 08-09 window**. Mean prediction of the shipped form moves
0.70607 → 0.70488 on all rows and 0.68852 → 0.68601 on scored rows (condition 7 figure, measured on
the shipped form rather than borrowed from another variant). Coverage scaling (condition 6): the
offset fires only on unseen objectives, so at the spec's ≥85% bound the expectation is
+0.00571 × 0.85 = +0.00486 before any transfer haircut.

Mechanism (condition 5), stated as what it is rather than what it flatters: a first-word-verb
authoring convention. Mean easiness by opening verb — `comparing` 0.769, `calculating` 0.716,
`working` 0.715, `finding` 0.709, `using` 0.707, `multiplying` 0.661, `adding` 0.647, `solving`
0.604 — against a global 0.7025.

### Slot plan for 08-09, three one-change comparisons against a common base

| slot | bundle | sha256 | the one change |
|---|---|---|---|
| 1 | `submission_ensemble.zip` | `288db042…be55a` | none — the ensemble has never been scored; it is the base and it settles ensemble-vs-`complex` against the 0.6162 anchor |
| 2 | `submission_ens_offset.zip` | `fe2e34a3a2f9f55677f6c8a2a7dd9352193e5ee1a67174adc3bd229f6d7e0cb6` | + struct difficulty offset, b = 0.20 |
| 3 | `submission_ens_textkey.zip` | (built 08-04) | difficulty table keyed on objective TEXT instead of id |

Slots 2 and 3 each differ from slot 1 by exactly one thing, so both are readable. δ = −0.08224 is
dropped from this window: the leaderboard's AUROC column showed our deficiency is discrimination,
not calibration, and δ is a pure prior move worth ~+0.0003 against the offset's +0.0057.

### Container validation of the offset bundle (job id-3732, 2026-08-04)

**Smoke score 0.4829 — identical to the id-keyed ensemble's 0.4829, exactly as pre-declared.** Smoke
rows are drawn from TRAIN, so their objectives are known, so the coverage gate must suppress the
offset and the bundle must reproduce its base. It did, in the real container, not only in a local
fixture.

The container log confirms the risky code actually ran rather than being skipped: the archive
unpacked 6 entries including `tta_objdiff.py` and `assets/objdiff_struct.json`, and the run printed
`wrote 100 rows (ensemble + struct objective-difficulty offset, b=0.2)` with exit code 0. So feature
extraction, the z-scoring, the ridge matrix multiply and the asset load are all validated in the
competition environment; the only branch never exercised there is the arithmetic that applies a
non-zero offset, which cannot be exercised by train-drawn smoke data and is covered by local parity
gate 4 (9.6e-02).

Residual risk, stated rather than buried: the first time an offset is actually added to a prediction
in the competition container will be on the scored run.

### Correction to the slot-2 expectation, made BEFORE the window (h10, 2026-08-04)

The pre-registered expectation of +0.00486 for the offset was **the maximum of nine partitions, not
its centre**. Measured across nine partitions the mean gain is **+0.00341**, so the development
figure of +0.00571 is optimistic by roughly a third — a partition factor of ×0.60–0.82. Applying it,
then ×0.85 for selection, ×0.93 for population and ×0.85 for coverage, with discrimination transfer
near 1.0:

**Revised pre-registered expectation for slot 2: +0.002 to +0.003 on the hidden test**, i.e. 0.6162
→ about 0.6132–0.6142. That is 19–26% of the 0.0118 gap to rank 15. The bundle is unchanged
(`submission_ens_offset.zip`, sha256 `fe2e34a3…0cb6`, container-validated as job id-3732); only the
number we predicted for it is corrected, and it is corrected downwards, in writing, before the run.

## Bundles re-issued 2026-08-04: hardened against the data-mount race

Forum topic 34 documents that the transcripts directory can be unready when inference starts, and
our bundles treated a missing transcript as an empty one and continued silently — a partial-data run
that returns plausible nonsense. `submission/harden_mount.py` injects a bounded poll before the
feature loop and logs what it ended up with; a run that proceeds on partial data now says so.

Parity gate: with every transcript present the hardened bundles reproduce the originals to
**0.00e+00**, so the change is behaviourally inert in the normal case.

| slot | bundle | sha256 |
|---|---|---|
| 1 | `submission_ensemble_v2.zip` | `e137e07cc721a9b5ce0efb6be2014a2a8ce7ad4c4d74726463e5f5a693b5bfb8` |
| 2 | `submission_ens_offset_v2.zip` | `63e59104b648bc24ca9b8ac41113abf5c606b6fa9015fc0d56c007b4283c96bd` |
| 3 | `submission_ens_textkey_v2.zip` | `592a162bbd04e72ab44251695f8a5e8639af73f5e145ef038cbacdedafd62018` |

The pre-registered predictions are unchanged, because the predictions themselves are unchanged.
Slot 3's rationale is now weaker on the organizer's own evidence — `learning_objective_id` is stable
across the whole dataset, so the text key cannot recover anything the id key misses. It stays only
because it costs nothing and because the ids-vs-text question is the last one we could still be
wrong about; if a better use for slot 3 appears before the window, take it.

Container validation of the hardened slot-2 bundle: **job id-3740, SMOKE_SCORE 0.4829, exit 0** —
identical to the unhardened bundle's 0.4829 and to the ensemble base, as required. The log contains
no `transcripts:` line, which is the intended silent fast path: every transcript was present on the
first check, so the poll printed nothing and cost nothing. The hardening only speaks when the mount
is late.

## Slot 3 reassigned, 2026-08-04, before the window

The text-key bundle no longer has a rationale. The organizer stated on 2026-06-30 that
`learning_objective_id` "always refers to the same learning objective … regardless of data source",
so keying on text cannot recover anything the id key misses, and the smoke test had already said the
same. Spending a scored slot to re-confirm a settled fact is the worst available use of one of nine.

**Slot 3 becomes the offset at b = 0.50**, `submission_ens_offset_b050_v2.zip`, sha256
`ac0315a38cea9972c6542e42ae389f34841ab1dc1bf16e17452664dc120330bb`, four parity gates passed
(b=0 reproduces the ensemble 0.00e+00; shipped dhat matches the fitted dhat 0.00e+00; the coverage
gate does not fire on known objectives 3.3e-16; it does fire on unknown ones 2.4e-01) and the
mount-hardening verified inert at 0.00e+00.

**Why this is legitimate and δ was not.** δ hard-coded a constant *inferred from a leaderboard score*,
which the organizer ruled violates independent processing. This ships two models that were both
fixed from TRAIN data alone, and reads the difference between their scores. That is ordinary
experimental design, not test-set inference — the same distinction the organizer drew when they said
centring on the train base rate "would be acceptable per the rules".

**What the pair measures.** On the clean ruler the offset gains +0.00571 at b = 0.20 and +0.00723 at
b = 0.50, with AUROC +0.0194 and +0.0231 — the ruler prefers the larger coefficient, but b = 0.20 was
pre-declared before measurement and stays the primary. Two points on the real test tell us whether
the response curve behaves as the ruler says. If the offset is real, slot 3 beats slot 2; if the
ruler's b-preference is an artefact of its cold-row population, slot 3 is worse and we learn that the
transfer is weaker than the proxy claims.

**Pre-registered predictions, written before the window:** slot 2 (b = 0.20) at **+0.002 to +0.003**
against slot 1, and slot 3 (b = 0.50) at **+0.002 to +0.004** — wider, because a larger coefficient
amplifies both the signal and any miscalibration. If slot 2 and slot 3 both land within ±0.001 of
slot 1, the offset does not transfer at all and the remaining windows should not spend anything more
on it.

## Pre-declaration: confirmation test of the inverted "demonstrated mastery" signal

Written before the confirmation run. The screen of six prompt variants on 400 rows found five of six
BELOW chance — not the shape of noise — with variant **B ("did the student demonstrate mastery?")**
at AUROC 0.4240, z = −2.34 against a correct null SE of 0.0325. Inverted, B gives **AUROC 0.5760**,
which is the level of our entire shipped ensemble on the real test (0.5750). It is not merely a
tutor-praise detector: its correlation with `tf_praise_rate` is −0.24.

Six variants were screened, so the multiplicity-corrected threshold is about |z| = 2.4 and B does not
clear it. That is exactly what a confirmation run is for.

**Declared before running:** variant B only, **n = 1200**, seed 7 (rows disjoint in expectation from
the screen's seed 20260804), transcript tail 1000 characters, Qwen2.5-0.5B-Instruct, the same
one-forward-pass extraction. One hypothesis, one test, no search, so no multiplicity correction
applies to the result.

**Decision rule, fixed now:** the inverted signal is real if AUROC > 0.5 with z > 3.0 on the fresh
sample. Between 2.0 and 3.0 it is "unresolved, worth one container variant". Below 2.0 the lane is
closed and no further compute goes to it. The increment over the shipped ensemble's own OOF on the
same rows is reported alongside, because beating chance is not the bar — adding something to what we
already ship is.

---

# AMENDMENT 2026-08-05 — the trio changed; the original predictions above stand as history

*The predictions above were frozen before the δ/offset plan collided with a rules ruling. They are
NOT deleted — a pre-registration whose value is its earliness must keep its original text. This
section records what changed, why, and the new frozen trio. The Saturday audit reads BOTH.*

## Why the δ-slots were withdrawn (rule quote)

The organizer ruled on the forum (2026-07-20, answering our exact construction): **inferring a
constant from a leaderboard score and hard-coding it violates the rules.** Centring on the TRAIN
base rate remains explicitly allowed; the δ = −0.08224 / −0.16448 offsets were derived from the
hidden-test anchor (constant 0.7025 → 0.6238), so they are non-compliant. All three δ bundles are
therefore QUARANTINED in `submission/.quarantine/` and are not submitted. Killing our own
pre-registered plan when it hit the ruling is itself the CORRECTIONS-grade record, not a loss.

## The frozen trio (channel decision #106, ledger F35)

| slot | bundle | sha256 | frozen prediction | what it measures |
|---|---|---|---|---|
| 1 | `submission_ensemble.zip` (δ = 0) | `288db0427667e6538929356a137bf6a0adeda42967110935c5b25367292be55a` | **0.613 [0.607, 0.619]** (original prereg, unchanged) | ensemble-vs-`complex` against the recorded 0.6162 anchor |
| 2 | `submission/submission.zip` (81-feature) | `5f479f9c179f6311e0a736da5bcd6e6cb5bb81f744d1aab80c6fc43205583c60` | **envelope [0.6168, 0.6216], centre ~0.619** (F35) | out-of-family transfer: does the big model transfer? |
| 3 | HOLD | — | — | the write-up dictates the third question after slots 1–2 are seen |

**Slot-2 envelope provenance (F35), two independent routes that intersect:** (A) anchor-ruler proxy
0.599204 + observed Δ 0.020015 → 0.619219 [0.616794, 0.621644]; (B) clean-cold proxy 0.657552 +
n = 2 cold-Δ −0.0401 ± 2e-4 → [0.617361, 0.617551]. Route B's narrowness is Δ-agreement, NOT
prediction certainty; n = 2 from one model family → a range, not a CI. Full SHAs of the six OOF
artefacts are in `writeup_public/FACTS.md` (F35) and `h81_oof_result.md`.

**Switch-rule note (channel #106):** an agent recommended swapping slot 2 to an ensemble variant on
an "all central estimates worse" tendency; this was OVERRIDDEN because the preregistered switch rule
(CI entirely worse than slot 1's [0.607, 0.619]) did NOT fire under the correct anchor convention.
Slot value here is INFORMATION, not board rank (we are rank 137): slot 2 tests the transfer function
out-of-family, and "the big model does not transfer" is recorded NOW as a prediction and Saturday as
a measurement — the framework graduates either way.

**Result recording:** whichever way each slot lands, it goes into the same table as the prediction —
a hit graduates the anatomy to a predictive instrument, a miss refutes it; both are published.

---

# RESULTS 2026-08-09 (recorded against the frozen trio above)

Window opened early: quota was already available at 03:53 UTC Saturday (the rolling-7d theory said
~14:47 UTC; measured, not assumed). Both bundles submitted with the new-id check; ids are fresh.

| slot | bundle (sha verified before submit) | prediction | RESULT | verdict |
|---|---|---|---|---|
| 1 | `submission_ensemble.zip` (288db042…) | 0.613 [0.607, 0.619] | **0.6141** (id-4351) | **HIT** — inside the envelope |
| 2 | `submission/submission.zip` 81-feat (5f479f9c…) | envelope [0.6168, 0.6216], centre ~0.619 | **0.6146** (id-4352) | **MISS, good side** — transfers better than the anatomy predicted |
| 3 | HOLD | — | — | 1 submission left this week |

What the numbers say:
* The ensemble (0.6141) beats `complex` (0.6162) by 0.0021 — the ensemble-vs-complex question is
  settled in the ensemble's favour and 0.6141 is the new best score.
* slot2 − slot1 = **+0.0005**: the 81-feature model is essentially AT the ensemble, not the
  ~+0.006 worse the F35 envelope predicted. Both envelope routes (anchor-ruler +Δ, clean-cold +Δ)
  overestimated out-of-family degradation. Per the freeze: a miss refutes the anatomy as a
  predictive instrument at this precision — that refutation is the ch4 branch to write, and it is
  publishable exactly as pre-committed ("both are published").
* Top-15 cutoff 0.6044 remains 0.0097 away; nothing in this window changes the "modelling path
  effectively closed" assessment.
