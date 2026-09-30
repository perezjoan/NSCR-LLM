# Code

`network_catchment_demo.ipynb` is the single notebook described in Section 3.7 of the
paper. It is organised in three parts that mirror the method: the deterministic
retrieval stage, the language-model stage, and the benchmark runner.

## Stage 1, deterministic (cells 1 to 10)

An interactive map (ipyleaflet) lets you click a point and set a walking distance `d`
and block depth `b`. The notebook then:

1. downloads the walkable street network around the point with OSMnx and projects it
   to a local metric CRS;
2. snaps the point to the nearest node and runs shortest-path traversal (NetworkX) with
   segment length as cost; segments only partly within budget are cut at the exact
   remaining distance (edge-based catchment);
3. buffers the reachable segments by `b`, unions them and closes gaps to obtain the
   filled catchment polygon;
4. streams GHS-POP 2021 (100 m, Cloud-Optimized GeoTIFF on OpenLandMap S3) through
   rasterio over the polygon only, with areal weighting per cell;
5. queries OpenStreetMap amenities, shops and leisure features (representative point
   inside the polygon) and building footprints (centroid-in, building parts and
   footprints below 15 m2 removed, coverage ratio clipped to the boundary);
6. reverse-geocodes the point with Nominatim and assembles the spatial brief as a JSON
   object, with a copy/paste cell to freeze or reload a brief.

## Stage 2, language model (cells 11 to 16)

- **Model table.** All sixteen configurations of Table 1 are selectable: Qwen3 1.7B,
  4B, 8B and 14B in thinking and non-thinking mode (same weights,
  `enable_thinking` toggled in the chat template), Gemma 4 12B in both modes,
  Gemma 3 1B, 4B and 12B, and Llama 3.2 1B and 3B and Llama 3.1 8B. Gemma and Llama
  repositories are gated; a Hugging Face login cell is provided (never hard-code a
  token in the notebook).
- **Loading.** 4-bit NF4 with double quantisation and bfloat16 compute for every model;
  Gemma 4 is loaded through its multimodal class and processor.
- **Sampling.** Vendor-recommended parameters per family are built in and applied
  automatically: Qwen3 0.6/0.95/20 (thinking) and 0.7/0.8/20 (non-thinking), Gemma 3
  and Gemma 4 1.0/0.95/64, Llama 0.6/0.9 with the library default for top-k.
- **Prompt.** Persona, glossary and brief are assembled into the system prompt. The
  default glossary constant is the frozen text (SHA-256 prefix `fa7ee737a14a`, matching
  the run metadata); the persona is pasted into a widget and hashed the same way. For
  Gemma 3, which has no system role, the system prompt is folded into the first user
  turn.
- **Generation.** Multi-turn with memory, fixed seed, 10,000-token default cap.
  Qwen3 `<think>` blocks and the Gemma 4 thought channel are separated from the answer,
  and only the answer re-enters the history. Truncation is detected by hitting the cap.

## Stage 3, benchmark runner (cells 17 to 20)

Replays Q1 to Q3 over seeds 1 to 10 for the loaded model, resetting memory between
seeds and keeping it within a seed, and appends one JSON record per response to a
per-model `.jsonl` file (schema in `../benchmark/runs/README.md`). It records the
glossary and persona hashes, the sampling parameters and their source, generation time
and the truncation flag. Truncated answers of 8,000 characters or more are treated as
degenerate repetition: the record is kept and the rest of that seed is aborted with
explicit aborted records, which is how the two Paris sequences of Llama-3.2-1B were
handled. Runs resume from an existing file, and outputs sync to Google Drive on Colab
or through rclone elsewhere.

The released run files in `../benchmark/runs/` were produced with this runner; they
predate the addition of the `degenerate` field to the record, which is otherwise
identical.

## Reproducing the frozen setting

1. Run Stage 1 once, or paste a brief from `../briefs/` into the load cell.
2. Load a configuration in Stage 2.
3. In Stage 3, paste the matching persona and the three questions from
   `../briefs/personas.txt`, set the case name (`chicago_800m`, `paris_400m`,
   `hanoi_300m`) and run. The printed glossary and persona hashes should read
   `fa7ee737a14a` and `e9ad438171dd` / `42faa0374a4b` / `23bd3f7063ef`.

## Environment

```bash
pip install -r ../requirements.txt --extra-index-url https://download.pytorch.org/whl/cu128
jupyter lab network_catchment_demo.ipynb
```

`requirements.txt` pins the versions reported by pip in the Colab sessions that
produced the paper's materials (Python 3.12, CUDA 12.8).

Stage 1 needs internet access (Overpass, Nominatim, OpenLandMap). Nominatim asks for at
most one request per second and a descriptive user agent. Stages 2 and 3 need a CUDA
GPU; the 12B to 14B models fit in roughly 10 GB of VRAM at 4-bit. Gemma 4 requires
transformers 5.10.1 or later. Weights are downloaded from the Hugging Face Hub on first
load after accepting the vendor licence.

Cell outputs, Jupyter widget state and Colab session metadata were stripped from the
notebook before it was added here; the cell sources are unchanged.
