# Annotation guides (version 1.0)

Four documents, all frozen at version 1.0 before annotation began and given unchanged to
the human annotator and to the language-model judge.

| File | Scope |
|---|---|
| `claim_annotation_guide.md` | General guide: the four labels, the two-pass decision procedure (source, then correctness), rules for derived numbers, evaluative claims, absence claims, hedging, echoes and location statements, and worked examples. |
| `chicago_annotation_guide.md` | Chicago case guide: frozen brief and glossary verbatim, persona and questions, sheet format, and Section 5 with the case-specific calls (no supermarket, the density call at 5,224/km2, the 16.7 POI ratio, recurring inventory misreads, how to label Q3). |
| `paris_annotation_guide.md` | Paris case guide, same structure (density call at 26,776/km2, verified per-category counts, the six excluded records). |
| `hanoi_annotation_guide.md` | Hanoi case guide, same structure (the residential caveat and how the models invert it, the people-per-building ratio, schema inversions). |

## Label vocabulary

The general guide names the categories `grounded`, `mistake`, `recall`, `hallucination`.
The city guides and every label file use the one-letter codes:

| Letter | Name | General-guide name |
|---|---|---|
| T | brief-true | grounded |
| F | brief-false | mistake |
| R | recall | recall |
| H | hallucination | hallucination |

The precedence rule is the same everywhere: the brief wins. A claim contradicting a brief
field is F when framed as reading the data and H when asserted as fact about the world.

## Sheet format

Claims arrive pre-segmented, one per line:

```
NN { } [claim text]
NN ECHO text            <- not a claim; questions to the user, offers of further analysis
```

Annotators write exactly one letter inside the braces and never alter the brackets.

## Notes

- The city guides were exported from a word processor, so some underscores in field names
  appear backslash-escaped (for example `poi\_total`). They are reproduced as given to the
  annotators rather than cleaned.
- The Q3 trap verdict (0, 0.5, 1 per seed) is scored separately and is not part of these
  guides; the rubric is stated in Section 4.2 of the paper and the per-seed justifications
  are in `../../results/Q3_trap_scores.tsv`.
