# Chapter 4½ — Five ways of asking whether effort shows from outside
*Draft v1 (2026-08-08). Every number traces to FACTS.md (F18, F37–F52); audit before
publish. Placement provisional: written to sit after chapter 4's reveal and before
chapter 5's zoom-out, so it doesn't interrupt the frozen-bet suspense and instead
supplies a second, independent pillar of evidence for the same argument — but it does
not depend on chapter 4's number and could stand on its own if the sequence changes.
Self-audit run 08-08: every numeric literal below checked against its FACTS.md row by
hand (no predictions+labels record exists for these summary statistics, so
provenance_audit.py's P1–P12 probes don't apply here — the ledger cross-check is the
audit for this chapter, same as ch1–ch3).*

A teacher watching a student hesitate over a problem can often tell, before checking
the answer, that something is wrong. The pause is too long, or too short — a fast
answer to a question with a well-known wrong shortcut reads differently than a fast
answer to an easy one. I wanted to know if that intuition could be measured from
outside, the way the rest of this project measures things: not by asking whether it
sounds right, but by asking whether it predicts anything a colder baseline doesn't
already predict.

I tried five ways, in increasing order of how hard they were to fake. The cheapest
first: hand a small local model — no internet, no API cost, one forward pass per
question — the bare text of each of the 398 learning objectives in this dataset and
ask it to judge, on five different phrasings, how much reasoning a typical problem on
that topic would demand. Five honest questions deserve five honest scores; instead
they came back as one score wearing five outfits. The five phrasings agreed with each
other at 0.74 to 0.91 — almost as if they were the same question — while agreeing
with which objectives were actually hard at 0.469 to 0.531 AUROC, indistinguishable
from a coin. A model that answers five different questions identically isn't reasoning
about five different things. It found one shallow feature of the text — probably
something like topic-label length or vocabulary rarity — and reported it back under
five names.

Maybe the model was too small to reason at all, so I asked a larger, genuinely
reasoning model — Kimi, run separately from anything scored here, purely as a second
opinion — to work through a representative problem for each objective step by step
and report how many reasoning steps it needed, whether it had to hold several values
in mind, and whether a plausible wrong shortcut existed. Token budgets ate more than a
third of the sample before I fixed them, leaving thirteen usable objectives out of
twenty attempted — not a content failure, just an infrastructure one worth naming
rather than hiding. On what survived: Spearman correlation between the model's rated
complexity and real empirical difficulty was −0.102 (p = 0.740); between step count
and difficulty, −0.047 (p = 0.880). And the near-miss-trap question, the one closest
to the teacher's intuition I started with, came back "yes" on all thirteen — a
question so leading that every answer was the same one, which is a failure of the
question, not evidence about the students.

I asked the same model to read my own results back to me before I believed them, and
it did the useful thing: it didn't accept the correlation test as the right bar. Raw
pass rate, it pointed out, is a noisy, selection-biased proxy for difficulty — exactly
the mechanism, it turns out, behind an older null result in this same project, where a
perfect oracle on a student's ability from their own other answers was worth
essentially nothing. The right question wasn't whether the LLM's score correlates with
that noisy proxy; it was whether adding the score to a model that already knows the
noisy proxy improves anything held out. So I built that test — five-fold grouped
cross-validation, the objective's own historical difficulty encoded strictly
out-of-fold, exactly the discipline the rest of this write-up insists on — and ran it
myself rather than asking the critic to grade its own homework. Baseline: log loss
0.5550, AUROC 0.7056. With the five LLM scores added: log loss 0.5549, AUROC 0.7055.
The difference, +0.00011 of log loss, sits inside its own standard error of 0.00013.
The sharper test agreed with the cruder one.

Two more angles, cheaper to build because they didn't need any model at all. If a
student answers within two seconds of being asked, are they guessing; if they take
five, did they catch something? Restricted to the 63.3% of sessions with exactly one
graded response — the only slice where a transcript's final exchange can be tied to a
specific answer without ambiguity — response latency correlated with correctness at
+0.0033 (p = 0.7711), and the same predictive-utility test that had already rejected
the LLM score rejected this too: log loss moved by −0.00029, the wrong direction,
smaller than its standard error of 0.00024. Spelling errors in the answer text told
the same story, weaker still: −0.0191 (p = 0.0965) raw, and after restricting to
answers with enough words that a dictionary check meant something, a predictive-utility
delta of −0.00044 — again wrong-signed, again inside the noise.

Five different instruments, aimed at the same idea from five different angles, and the
same answer came back five times. None of it changes what chapter 2 already showed:
almost everything this dataset has to say about a response sits in which objective was
asked, not in anything visible about how the answer was produced. An oracle on the
objective is worth +0.05367 of skill; the best oracle anyone could build from a
student's own surrounding behavior, in this project's earlier measurement, was worth
about zero. Five failed attempts to find a workaround around that ceiling is not a
disappointing result. It's the same ceiling, measured from five more directions, by
someone who kept trying to find a hole in it.
