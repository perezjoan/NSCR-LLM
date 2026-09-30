# Benchmark materials

The benchmark corpus flows through three stages, one folder each, plus the calibration
subset and the guides that govern labelling.

```
runs/      raw model outputs          16 configurations x 3 cities x 10 seeds x 3 questions = 1,440 records
  |
  v  exhaustive segmentation into atomic claims (Section 3.6 of the paper)
claims/    printable claim sheets     one file per configuration and city, all ten seeds, labels blank
  |
  v  labelling against the frozen brief + glossary under annotation_guide/
labels/    judge labels               one file per configuration and city, all ten seeds, T / F / R / H

seed1_human_vs_machine/   the seed-1 subset of claims/ (anonymised M01-M16), labelled twice:
                          by the human annotator and by the language-model judge
annotation_guide/         the general guide and the three city guides (v1.0) given to both annotators
```

## Naming

Configuration names are shared by `runs/`, `claims/` and `labels/`:

| Name | Checkpoint | Mode |
|---|---|---|
| `Qwen3-{1.7B,4B,8B,14B}_thinking` | Qwen/Qwen3-* | thinking on |
| `Qwen3-{1.7B,4B,8B,14B}_no-think` | Qwen/Qwen3-* | thinking off |
| `Gemma-4-12B-it_thinking` | google/gemma-4-12b-it | thinking on |
| `Gemma-4-12B-it_no-think` | google/gemma-4-12b-it | thinking off |
| `Gemma-3-{1B,4B,12B}-it` | google/gemma-3-*-it | n/a |
| `Llama-3.1-8B-Instruct` | meta-llama/Llama-3.1-8B-Instruct | n/a |
| `Llama-3.2-{1B,3B}-Instruct` | meta-llama/Llama-3.2-*-Instruct | n/a |

Case identifiers are `chicago_800m`, `paris_400m` and `hanoi_300m`. Questions are Q1
(retrieval), Q2 (reasoning), Q3 (planted false premise). Seeds run from 1 to 10.

In the seed-1 packs the configurations are anonymised as M01 to M16; the mapping is in
`seed1_human_vs_machine/model_code_table.md`.

## Counts

| City | Records in `runs/` | Valid | Claims in `claims/` | ECHO lines | Seed-1 claims double-labelled |
|---|---|---|---|---|---|
| Chicago | 480 | 480 | 6,187 | 53 | 606 |
| Paris | 480 | 474 | 9,154 | 28 | 931 |
| Hanoi | 480 | 480 | 6,754 | 14 | 666 |

The six invalid Paris records are the two truncated Q1 answers of Llama-3.2-1B (seeds 1
and 9) and their four aborted follow-ups; they are excluded from the claim sheets and
from all measures.
