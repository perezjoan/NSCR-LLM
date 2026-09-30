# Model code correspondence (M01 to M16)

Codes are stable across all three cases (Chicago, Paris, Hanoi). Assignment was a deterministic shuffle (rng seed 20260729) of the alphabetical file order, so code order carries no information about model identity.

| Code | Model configuration       |
|------|---------------------------|
| M01  | Gemma-3-12B-it            |
| M02  | Qwen3-1.7B (thinking)     |
| M03  | Qwen3-1.7B (no-think)     |
| M04  | Llama-3.1-8B-Instruct     |
| M05  | Qwen3-8B (thinking)       |
| M06  | Qwen3-4B (no-think)       |
| M07  | Gemma-3-1B-it             |
| M08  | Gemma-3-4B-it             |
| M09  | Qwen3-8B (no-think)       |
| M10  | Gemma-4-12B-it (thinking) |
| M11  | Gemma-4-12B-it (no-think) |
| M12  | Qwen3-14B (thinking)      |
| M13  | Llama-3.2-1B-Instruct     |
| M14  | Llama-3.2-3B-Instruct     |
| M15  | Qwen3-14B (no-think)      |
| M16  | Qwen3-4B (thinking)       |

Machine-readable copy of this mapping: model_code_map.json (kept alongside the working files, not in the annotation folder).


Blinding note: this mapping was withheld from both the human annotator and the language-model judge while the seed-1 packs were labelled (the packs identify configurations only by code). It is released here, after annotation, so that the seed-1 label files can be joined to the model names used everywhere else in the repository. The machine-readable copy mentioned above (model_code_map.json) is not included in this snapshot; this table is the reference.
