# Atomic claim sheets (unlabelled)

One file per configuration and city, containing all ten seeds and all three questions of
that configuration, segmented into atomic claims with blank braces. These are the
printable annotation sheets from which `../labels/` were produced.

## Format

```
##### chicago_800m | Qwen3-8B_no-think | printable annotation sheet | labels: T (brief-true) F (brief-false) R (recall) H (hallucination) | ECHO lines are not claims

=== chicago_800m | Qwen3-8B (no-think) | seed 1 | Q1 ===
01 { } [In this area of Chicago, the catchment includes a variety of food retail options reachable on foot.]
02 { } [However, the only explicit food retail categories listed are "convenience" (4 locations)]
...
07 ECHO Would you like me to explore what a supermarket would add to this catchment?
```

- A `===` line opens each response block: case, configuration, seed, question.
- `NN { } [text]` is one atomic claim. Numbering restarts at 01 in every block.
- `NN ECHO text` is a non-claim (a question back to the user, an offer of further
  analysis, a statement that more data would be needed). ECHO lines carry no braces and
  are excluded from every count.
- Model formatting (asterisks, inline headers, typos) is preserved verbatim inside the
  brackets.

## Segmentation conventions

An atomic claim is the smallest span making an independently checkable assertion: a
number, count, existence statement, characterisation or conclusion. Figures are separated
from the interpretations drawn from them, distinct items in enumerations are split, and
collective characterisations are kept whole when the assertion concerns the mix rather
than its members (Section 3.6 of the paper).

## Counts

| City | Files | Claims | ECHO lines |
|---|---|---|---|
| chicago | 16 | 6,187 | 53 |
| paris | 16 | 9,154 | 28 |
| hanoi | 16 | 6,754 | 14 |

The Paris sheet for Llama-3.2-1B covers 24 responses rather than 30: the two truncated
Q1 answers (seeds 1 and 9) and their four aborted follow-ups are not in the sheet.
