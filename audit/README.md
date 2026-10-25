# Audit data

The raw data behind the OCR audit benchmark in the root README. Every figure in
that section can be recomputed from these files.

| file | rows | contents |
|---|---:|---|
| `gemini_audit.tsv` | 16,149 | every correction proposed during the audit, with the native speaker's verdict |
| `print_errors.tsv` | 67 | misprints in the printed dictionaries themselves |

Both files are generated from the dictionary workbooks by `export_audit.py`
and are not edited by hand.

## How the audit worked

All three dictionaries were audited with `gemini-3.1-pro-preview`. The model
read each scanned page column alongside the rows OCR had produced from it and
proposed corrections. A native speaker then checked every proposal against the
scan and accepted or rejected it. The audit scripts were adjusted for each
dictionary's layout and era, so a few columns exist only for some dictionaries;
they are left empty elsewhere.

## `gemini_audit.tsv`

One row per proposed correction.

| column | meaning |
|---|---|
| `dictionary` | work and volume, e.g. `izahliq_lughet_1990s_v3` |
| `image` | the scanned column image the proposal refers to |
| `row_id` | the entry the proposal concerns |
| `issue` | the model's error class, e.g. `wrong_characters`, `missing_headword` |
| `field` | which part of the entry the proposal changes |
| `excel_text` | the text as OCR produced it |
| `image_text` | the text the model read from the scan, i.e. its proposed correction |
| `explanation` | the model's own explanation, verbatim (English) |
| `confidence` | the model's self-reported confidence |
| `decision` | the native speaker's verdict: `accept` or `reject` |
| `finder` | who proposed it: `gemini`, `script` or `claude` (see below) |
| `attestation_class` | 1990s volumes 1–5 only: whether each form occurs in that volume's definitions |
| `note` | reviewer and script notes made during the audit |
| `applied` | what was done to the list as a result |

**Only `gemini` rows are part of the benchmark.** The other rows were found by
separate checks run alongside the audit: 112 `script` rows in the 2009
dictionary (stray parenthetical text, generated mechanically) and 40 `claude`
rows in the 1990s volumes (mostly possible lost entries). They are kept so the
file is the complete record of what was reviewed.

**Only `accept` and `reject` count.** 63 rows in the 2009 dictionary carry no
verdict or a different note, because the error had already been corrected
before the proposal was reviewed.

To reproduce the benchmark table:

```python
import csv
from collections import Counter

rows = csv.DictReader(open("gemini_audit.tsv", encoding="utf-8"),
                      delimiter="\t", quoting=csv.QUOTE_NONE)
total, accepted = Counter(), Counter()
for r in rows:
    if r["finder"] == "gemini" and r["decision"] in ("accept", "reject"):
        total[r["dictionary"]] += 1
        accepted[r["dictionary"]] += r["decision"] == "accept"
for d in total:
    print(d, total[d], accepted[d], f"{100 * accepted[d] / total[d]:.1f}%")
```

This gives 15,934 proposals and 9,560 accepted (60.0%), as in the root README.

## `print_errors.tsv`

Headwords that are misspelled in the printed book itself, not by OCR. Each was
confirmed against the scan during the audit. The published lists use
`correct_form`. Fifteen rows in the 2009 dictionary are sub-entries; the note
column says so.

| column | meaning |
|---|---|
| `dictionary` | work and volume |
| `row_id` | the entry in the published list |
| `printed_form` | the headword as printed |
| `correct_form` | the corrected form, as published |
| `source_page` | the scanned column image |
| `note` | reviewer's note, where one was made |

These 67 rows are in the 2009 dictionary (19), the 1990s dictionary (43),
2011 volume 1 (1), and 2011 volume 2 (4). The 1990s rows are in volume 1 (23),
volume 3 (4), volume 4 (5), volume 5 (2), and volume 6 (9). 1990s volume 2
has none.

## This is a snapshot

`row_id` and `image` refer to the lists as they stood at audit time. Rows the
audit later removed, or that were later excluded as Chinese transliterations,
are still listed here. All verdicts are from the first audit round
(`status_v1`). If a later round overturns any, this file will be regenerated
and the change recorded in `CHANGELOG.md`.

The scanned images themselves are not published; filenames are given so that
anyone with access to the printed volumes can find the column in question.
