# Claim-level labels, all ten seeds (language-model judge)

One file per configuration and city, holding the judge's label for every claim in the
matching sheet in `../claims/<city>/<configuration>.txt`. Labels are T (brief-true),
F (brief-false), R (recall), H (hallucination), decided under the city guide in
`../annotation_guide/` (v1.0). Claims are identified by seed, question and the claim
number in the sheet. ECHO lines are never labelled.

Every file states the guide version and the glossary and persona hashes in its header;
several also end with a TOTALS block giving the four counts that feed Table C1 of the
paper. Where there is no such block, count the labels directly.

## File formats

The 48 files were produced in separate sessions and use six layouts. All carry the same
information; a consolidation script is a planned addition (see the checklist in the root
README). The layouts are:

**A. Full sheet with labels in place** (claim text retained, optional `#` note after
the claim):

```
=== chicago_800m | Qwen3-8B (no-think) | seed 1 | Q1 ===
01 {F} [In this area of Chicago, the catchment includes a variety of food retail options reachable on foot.]
02 {T} [However, the only explicit food retail categories listed are "convenience" (4 locations)]  # note
```

**B. One block per response, one claim per line**, label after the number, optional
free-text note:

```
=== seed 1 | Q3 ===
01 R  meta
02 T
04 F  7131 + 50.3 read as small resident population / commercial dominance (5.2, 5.5)
```

**C. One row per response, labels in claim order:**

```
seed 1 | Q1: T T F F
seed 1 | Q2: T T T T T T R T T F R T F F R T T T T T F T R T T T
```
or
```
seed 1 | Q1 | T T T T T T T R T T T T T T T T R
  08 R designed-for inference; 17 R quality-of-life judgement
```

**D. Inline numbered labels** with indented notes:

```
=== seed 1 | Q1 ===
01 {T} 02 {R} 03 {T} 04 {T} 05 {T}
  note 13: ranks unit-incommensurable metrics against each other
```

**E. Pipe-separated with parenthetical notes:**

```
seed1 Q1: 01 T | 02 T | 03 F (conv 4 read as 1) | 04 R | 05 T
```

**F. Space-separated numbered labels** with a claim count in the block header:

```
=== seed 1 | Q1 (12) ===
01 T  02 T  03 T  04 T  05 T  06 T  07 T  08 T  09 T  10 T  11 T  12 T
```

In every layout the label sequence within a response follows the claim numbering of the
sheet, and where a response has a gap in numbering (a claim skipped for a documented
reason) the file says so next to the row.

## Consistency with the paper

Per-configuration label counts in these files are the counts reported in Table C1 of
the paper (for example Qwen3-8B no-think in Chicago: 335 claims, T 219, F 59, R 57,
H 0, which is what counting the braces in that file gives). Absolute
error rates should be read with the judge's slightly strict convention in mind
(see `../seed1_human_vs_machine/README.md`).
