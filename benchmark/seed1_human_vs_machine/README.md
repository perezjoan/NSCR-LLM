# Seed 1: human annotator versus language-model judge

Labelling the full ten-seed grid by hand was impractical, so the paper uses a
language-model judge calibrated against a human annotator on seed 1 of every
configuration in every city. This folder holds that calibration set.

## Files

```
model_code_table.md          M01-M16 to configuration name (withheld during annotation)
chicago/
  chicago_seed1_annotation_pack.txt      blank pack (reconstructed, see below)
  chicago_seed1_labels_human.txt         human labels, in the sheet
  chicago_seed1_labels_machine.txt       judge labels, in the sheet, with header notes
  annotator_chicago.txt                  judge model, session date and conditions
  chicago_validation_results.md/.docx    agreement analysis for Chicago (kappa, confusion matrix, disagreement loci)
paris/
  paris_seed1_annotation_pack.txt        blank pack as given to both annotators
  paris_seed1_labels_human.txt           human labels, in the sheet
  paris_seed1_labels_machine.txt         judge labels, one label per claim number, no claim text
hanoi/
  hanoi_seed1_annotation_pack.txt        blank pack as given to both annotators
  hanoi_seed1_labels_human.txt           human labels, in the sheet
  hanoi_seed1_labels_machine.txt         judge labels, in the sheet, notes collected at the end
```

Each pack contains, for the 16 anonymised configurations, the seed-1 conversation (Q1 to
Q3) segmented into atomic claims, with blank braces to fill. In Paris, configuration M13
(Llama-3.2-1B) contributes its seed-2 conversation because its seed-1 records are among
the six excluded degenerate records.

The Chicago blank pack was not kept separately in the working files; the copy here was
reconstructed by blanking the labels of the human sheet. Its content is otherwise the
sheet both annotators received.

## Procedure

- Both annotators worked from the same city guide (`../annotation_guide/`, v1.0) with
  model identities hidden behind the M-codes.
- Judge: Claude Fable 5.1 (Anthropic, `claude-fable-5-1`, knowledge cutoff June 2026),
  accessed through the Claude chat interface, memory features off, no web search, one
  pass per city with the guide and the full pack in context. Session date for all three
  cities: 13 September 2026 (see `chicago/annotator_chicago.txt` and Section 4.3 of the
  paper).
- Claims compared: 606 (Chicago), 931 (Paris), 666 (Hanoi), 2,203 pooled. ECHO lines
  are excluded.

## Agreement (Table 2 of the paper)

| | Chicago | Paris | Hanoi | Pooled |
|---|---|---|---|---|
| Claims compared | 606 | 931 | 666 | 2,203 |
| Raw agreement | 86.6% | 80.7% | 89.6% | 85.0% |
| Cohen's kappa, 4 labels | 0.709 | 0.615 | 0.770 | 0.685 |
| 95% CI of kappa | 0.65-0.76 | 0.57-0.66 | 0.72-0.82 | 0.66-0.71 |
| Kappa, acceptable (T, R) vs error (F, H) | 0.81 | 0.66 | 0.79 | 0.74 |
| Kappa by question, Q1 / Q2 / Q3 | 0.74 / 0.62 / 0.73 | 0.67 / 0.64 / 0.50 | 0.83 / 0.85 / 0.62 | 0.72 / 0.68 / 0.62 |
| Per-configuration error-rate correlation, Spearman rho | 0.96 | 0.93 | 0.85 | 0.96 |

Pooled Krippendorff's alpha is 0.684. Collapsing to acceptable versus error raises
pooled agreement to 91.6 percent.

## Where the two disagree

The 330 disagreements concentrate in three patterns:

- **Brief-grounded inference versus recall.** Generic or hedged statements such as
  "convenience stores offer limited fresh produce" were T for the human and R for the
  judge (88 cases).
- **Endorsing the trap premise.** Brief-false for the human, hallucination for the judge
  (44 cases); both agree the claim is wrong and differ on framing.
- **Propagation.** A verdict resting on an earlier false claim was F for the human but R
  for the judge when assessed on its own terms (51 cases).

The judge assigned the more severe label in 65 percent of disagreements, so absolute
error rates for seeds 2 to 10 in `../labels/` should be read as slightly strict relative
to the human baseline.

## Reading the label files

Human sheets and the Chicago and Hanoi judge sheets keep the claim text:

```
01 { T } [This Chicago location has a substantial residential population of 7,128 residents]
```

The Paris judge file lists labels by claim number only; join it to the pack by
configuration, question and claim number:

```
=== M01 | seed 1 | Q1 ===
01 T
02 T
```
