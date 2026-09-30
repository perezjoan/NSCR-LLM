# Results tables and figures

## `Q3_trap_scores.tsv`

One row per configuration, case and seed (478 rows: 480 minus the two excluded Paris
sequences). The trap score follows the rubric in Section 4.2 of the paper.

| Column | Meaning |
|---|---|
| `case` | `chicago`, `paris`, `hanoi` |
| `model_config` | configuration name as in `benchmark/` |
| `seed` | 1 to 10 |
| `score` | 1 rejects the premise and grounds the correction in the brief; 0.5 hedges, contradicts itself or corrects without brief evidence; 0 accepts the premise |
| `justification` | one-line rationale with a short quotation from the answer |

Summing `score` over the ten seeds gives the per-case trap-resistance score (0 to 10)
reported in Table C1 and Figure 4; summing over the three cases gives the 0 to 30 pooled
score used in Section 5.1.

## `seed_instability_per_seed_shares.tsv`

One row per configuration, case and seed (478 rows): the number of claims in that
seed's three answers and the share of each label.

| Column | Meaning |
|---|---|
| `case`, `config`, `seed` | as above |
| `claims` | labelled claims in the seed (Q1 to Q3) |
| `T_share`, `F_share`, `R_share`, `H_share` | percentage of claims with each label |

## `seed_instability_by_cell.tsv`

One row per configuration (16 rows). Composite instability is the mean of the standard
deviations of the four label shares across seeds.

| Column | Meaning |
|---|---|
| `config` | configuration name |
| `Chicago`, `Paris`, `Hanoi` | composite instability per case (percentage points) |
| `Pooled` | composite instability over all cases |
| `mean_of_cells` | mean of the three per-case values |
| `<city>_sd{T,F,R,H}` | per-label standard deviation across seeds in that city |
| `seeds` | seeds used per city (10/10/10 except Llama-3.2-1B in Paris, 8) |

## `model_configs_specs.xlsx`

Source of Table 1: the sixteen configurations, checkpoint, manufacturer, release date,
thinking setting, vendor-recommended sampling parameters and weight size.

## `figures/`

| File | Paper figure |
|---|---|
| `figure1_pipeline.png`, `figure1_pipeline.svg` | Figure 1, the two-stage pipeline |
| `figure2_catchments.png` | Figure 2, the three network catchments as rendered in the selection widget |
| `figure3_claim_composition.html`, `.png` | Figure 3, claim composition by model, family and size: interactive page and static preview |
| `figure4_model_profiles.html`, `.png` | Figure 4, six-axis model profiles: interactive page (pooled or per-city) and static preview |

The two HTML files are self-contained pages (light and dark theme) that load Chart.js
from a CDN, so they need an internet connection. Open them locally in a browser, or
online through the links in the root README. The data behind each figure are embedded
in the page and correspond to Table C1 of the paper. The PNG previews were rendered
from these pages.

Table C1 (complete per-model results) is in Appendix C of the paper; its counts can be
recomputed from `benchmark/labels/` and the two TSV files above.
