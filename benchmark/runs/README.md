# Raw model outputs

One JSON Lines file per configuration and city, 30 records each (10 seeds x 3
questions), 48 files, 1,440 records in total. Runs were made between 16 and 28 July 2026
on a single GPU, all models loaded in 4-bit NF4.

## Record schema

```json
{
  "timestamp": "2026-07-24T07:12:47",
  "model_label": "Qwen3-8B (no-think)",
  "model_family": "qwen",
  "model_thinking": false,
  "case": "chicago_800m",
  "seed": 1,
  "q_index": 1,
  "question": "What food retail is reachable on foot here, and how many people live in the catchment?",
  "temperature": 0.7,
  "top_p": 0.8,
  "top_k": 20,
  "sampling_source": "vendor_recommended",
  "max_tokens": 10000,
  "glossary_sha256": "fa7ee737a14a",
  "persona_sha256": "e9ad438171dd",
  "gen_seconds": 36.4,
  "truncated": false,
  "answer": "In this area of Chicago, ..."
}
```

| Field | Meaning |
|---|---|
| `model_label` | display name of the configuration |
| `model_family` | `qwen`, `gemma`, `gemma4` or `llama` |
| `model_thinking` | whether the reasoning trace was enabled (Qwen3, Gemma 4) |
| `case` | `chicago_800m`, `paris_400m`, `hanoi_300m` |
| `seed` | 1 to 10; memory is reset between seeds and kept within a seed |
| `q_index` | 1 retrieval, 2 reasoning, 3 planted false premise |
| `temperature`, `top_p`, `top_k` | decoding parameters (Table 1 of the paper); `top_k` is null where the vendor specifies none |
| `sampling_source` | `vendor_recommended` for every record |
| `max_tokens` | generation cap, 10,000 for every record |
| `glossary_sha256`, `persona_sha256` | first 12 hex digits of the SHA-256 of the glossary and persona text used (`../../briefs/`) |
| `gen_seconds` | wall-clock generation time for this answer |
| `truncated` | true when the generation cap was reached |
| `answer` | the final answer only; reasoning traces were separated before logging and were not replayed into later turns |

## Exclusions

Two records have `"truncated": true`: `paris/Llama-3.2-1B-Instruct.jsonl`, seeds 1 and
9, question 1, where the model looped until the cap. Both sequences (six records) are
excluded from the claim sheets and from all measures in the paper; they are kept here
unchanged for completeness.
