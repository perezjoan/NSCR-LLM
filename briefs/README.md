# Frozen spatial briefs, personas and glossary

Everything in this folder is the fixed input side of the benchmark. The three briefs
were generated once by Stage 1 of the notebook, frozen, and presented unchanged to
all sixteen model configurations. OpenStreetMap and GHS-POP evolve, so rerunning the
notebook on the same coordinates today will give slightly different numbers; the
files here are the ground truth against which every claim label was decided.

| File | Case | Point (lat, lon) | Walking distance | Block depth | Residents | POIs |
|---|---|---|---|---|---|---|
| `chicago_800m.json` | Chicago, West Side (food access) | 41.8819, -87.7036 | 800 m | 40 m | 7,128 | 119 |
| `paris_400m.json` | Paris, 3rd/4th arr. near Le Marais (daily-needs walkability) | 48.8602, 2.3550 | 400 m | 40 m | 8,440 | 971 |
| `hanoi_300m.json` | Hanoi, Old Quarter near Hoan Kiem (urban form) | 21.0340, 105.8500 | 300 m | 40 m | 7,131 | 359 |

Each brief is a plain JSON object with the blocks `retrieval_method`, `location`,
`point`, `population` (with the residential-only caveat), `road_length_m`,
`poi_total` / `poi_by_category`, and `indicators`. The definition and computation
rule of every indicator is given in Appendix A of the paper. In all three files the
per-category POI counts sum exactly to `poi_total`.

## Personas and questions

`personas.txt` holds, verbatim, the three personas (system-prompt role text) and the
three questions asked under each. Persona A (food access) goes with Chicago, B
(daily-needs walkability) with Paris, C (urban form) with Hanoi. Question 1 retrieves,
question 2 reasons, question 3 plants a false premise that the brief refutes. The
persona hashes recorded in the run metadata are:

| Persona | Case | `persona_sha256` (first 12 hex) |
|---|---|---|
| A, food access | Chicago | `e9ad438171dd` |
| B, daily-needs walkability | Paris | `42faa0374a4b` |
| C, urban form | Hanoi | `23bd3f7063ef` |

## Glossary

`glossary.md` is the field glossary injected between persona and brief in every
prompt (`glossary_sha256` = `fa7ee737a14a`). Annotators were instructed to treat it
as part of the brief.

## Prompt assembly

The system prompt seen by a model was:

```
<persona text>

<glossary text>

SPATIAL BRIEF:
<brief JSON, indented>
```

followed by the three questions as successive user turns in one conversation with
memory. For Gemma 3, which accepts no system role, the same text was folded into the
first user turn. Reasoning traces (Qwen3 and Gemma 4 thinking modes) were separated
from the answer and not replayed into later turns.

## Provenance and licence

Indicator values derive from OpenStreetMap (ODbL 1.0, (c) OpenStreetMap contributors)
and GHS-POP R2023A (JRC, CC BY 4.0; 2021 epoch, 100 m grid), reverse-geocoded with
Nominatim.

**Retrieval date.** All three briefs were generated and frozen in one session on
**25 June 2026**, in the order Chicago, Paris, Hanoi. OpenStreetMap data (street
network, POIs, buildings) were fetched live through the Overpass API at that time, and
the Nominatim place names likewise. The frozen files were saved at approximately 11:07,
11:12 and 11:16 UTC on that date, minutes after each retrieval. The benchmark runs took
place between 16 and 28 July 2026 against these frozen briefs; no retrieval was repeated.
