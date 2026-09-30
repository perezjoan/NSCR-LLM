# Annotation guide validation: human reference versus an independent LLM rater

Chicago case (chicago_800m), seed-1 annotation pack. Working results for the
faithfulness benchmark, guide validation subsection. (Markdown rendition of
`chicago_validation_results.docx`; the tables are transcribed unchanged.)

## Setup

The seed-1 annotation pack for the Chicago case (16 anonymised model configurations,
M01 to M16; 3 questions each; 606 atomic claims; 5 ECHO lines excluded) was labelled
independently twice: by the human reference annotator and by Claude Fable 5.1 acting as
a fresh rater. The rater received exactly two files verbatim, the annotation guide
(version 1.0, containing the spatial brief and field glossary) and the blank pack, and
returned one label per claim from the set T (brief-true), F (brief-false), R (recall),
H (hallucination). Model codes were blinded in both conditions. All 606 claims were
labelled by both raters; agreement is computed on the full set.

**Table 1. Agreement between the human reference and Claude Fable 5.1 (n = 606 claims).**

| Subset | Agreement | % | 95% CI | Cohen's kappa |
|---|---|---|---|---|
| All claims | 525/606 | 86.6 | 83.7-89.1 | 0.709 (0.650-0.768) |
| Q1/Q2 (descriptive) | 380/430 | 88.4 | 85.0-91.1 | 0.668 |
| Q3 (trap) | 145/176 | 82.4 | 76.1-87.3 | 0.725 |

Confidence intervals for percentages are Wilson score intervals; the kappa interval is
asymptotic. Observed agreement 0.866; chance agreement 0.541. Raw agreement is lower on
the Q3 trap blocks but kappa is higher, because the Q3 label mix is less dominated by T
and therefore less agreement is attributable to chance.

**Table 2. Per-label agreement (share of reference-labelled claims matched by the rater).**

| Reference label | n | Rater matched | % | 95% CI |
|---|---|---|---|---|
| T (brief-true) | 453 | 404 | 89.2 | 86.0-91.7 |
| F (brief-false) | 82 | 58 | 70.7 | 60.1-79.5 |
| R (recall) | 38 | 35 | 92.1 | 79.2-97.3 |
| H (hallucination) | 33 | 28 | 84.8 | 69.1-93.3 |

**Table 3. Confusion matrix (rows: human reference; columns: Fable 5.1).**

| | T | F | R | H |
|---|---|---|---|---|
| **T** | 404 | 20 | 26 | 3 |
| **F** | 10 | 58 | 0 | 14 |
| **R** | 0 | 0 | 35 | 3 |
| **H** | 1 | 3 | 1 | 28 |

## Structure of the disagreements

The 81 disagreements are not randomly distributed. Sixty-six (81%) fall in loci
identified before the run as expected disagreement zones, either interpretive choices
the reference annotator pre-registered or borderline items flagged during a pre-run
review. A single interpretive choice, whether an external-knowledge relative clause of
the form "...which are essential for accessing healthy, affordable food" is labelled T
(as a characterisation anchored to brief values) or R (as imported general knowledge),
accounts for 26 of the 81 disagreements on its own; the rater consistently chose R where
the reference chose T. A further 17 disagreements are F/H swaps inside the Q3
capitulation tails, a boundary the guide explicitly tolerates in both directions (the
two labels agree the claim is wrong and differ only on framing). This concentration also
explains the lower per-label score for F in Table 2.

**Table 4. Disagreement loci (n = 81).**

| Locus | n | Direction (reference to rater) |
|---|---|---|
| Substitution and criticality clauses (external stock knowledge) | 26 | T to R |
| F/H boundary in Q3 capitulation tails | 17 | 14 F to H; 3 H to F |
| Verdict-strength adjectives (e.g. "moderately well-served") | 7 | mixed |
| Exhaustive "only" inventories under tolerated accounting | 6 | T to F |
| "Relatively dense/high" density adjectives | 5 | F to T |
| Items flagged in pre-run review; rater sided with the review | 5 | T to F or H |
| Unanticipated, no single pattern | 15 | mixed |

## Interpretation

Overall kappa of 0.709 falls in the conventional "substantial agreement" band,
supporting the claim that the annotation guide transmits the labelling scheme to an
independent rater. Because the disagreements concentrate in a small number of
identifiable interpretive choices rather than scattering across the pack, most residual
disagreement is attributable to guide underdetermination at known boundaries (the T/R
source test for evaluative bridges, and the F/H framing test in premise-adopting claims)
rather than to rater noise. These two boundaries are the natural targets for the next
guide revision.

**Caveats.** Single rater, single run, one case (Chicago); the rater model belongs to the
same family as the assistant that helped draft the guide, so this is a transmission check
rather than a fully independent validation; guide version 1.0 was supplied to the rater.
Claims: 606; ECHO lines excluded: 5.
