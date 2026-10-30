# Audit data

The raw data behind the OCR audit benchmark in the root README. Every figure in
that section can be recomputed from these files.

| file | rows | contents |
|---|---:|---|
| `gemini_audit.tsv` | 16,149 | every correction proposed during the audit, with the native speaker's verdict |
| `print_errors.tsv` | 225 | misprints in the printed dictionaries themselves |

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
`correct_form`. 154 rows in the 2009 dictionary are sub-entries; the note
column says so.

| column | meaning |
|---|---|
| `dictionary` | work and volume |
| `row_id` | the entry in the published list |
| `printed_form` | the headword as printed |
| `correct_form` | the corrected form, as published |
| `source_page` | the scanned column image |
| `note` | reviewer's note, where one was made |

These 225 rows are in the 2009 dictionary (177), the 1990s dictionary (43),
2011 volume 1 (1), and 2011 volume 2 (4). The 1990s rows are in volume 1 (23),
volume 3 (4), volume 4 (5), volume 5 (2), and volume 6 (9). 1990s volume 2
has none.

Most of the 2009 rows were found through the cross-check described below.

## `gheyret_crosscheck.tsv`

The 2009 imla dictionary has also been digitized independently by Gheyret
([gheyret/imlalughet](https://github.com/gheyret/imlalughet), MIT). Our list
was compared with his entry by entry: 1,357 of the 80,864 entries in the
dictionary (534 headwords, 823 sub-entries; 1.7%) are spelled differently.
Every difference was checked against the scan, and the scan decided.
His list served as a pointer to places worth checking, not as a source: no
spelling was taken from it without the scan confirming it. His spellings
appear here only as comparison values.

The comparison was made on 2026-10-25 and applied on 2026-10-29. The file is
frozen: `row_id`, `source_page` and `status_v1_at_check` describe the list as
it stood then.

| column               | meaning                                                      |
| -------------------- | ------------------------------------------------------------ |
| `row_id`             | the entry in the published 2009 list                         |
| `form`               | `headword` or `sub-entry`                                    |
| `status_v1_at_check` | the entry's `status_v1` when the comparison was made         |
| `source_page`        | the scanned column image                                     |
| `extracted`          | our spelling before the check                                |
| `gheyret`            | the spelling in Gheyret's list, as he has it                 |
| `resolution`         | what the scan showed (codes below)                           |
| `published_form`     | the spelling the published list carries after the check      |
| `note`               | evidence: the scan reading, and the same word in the 2009, 2011 or 1990s lists (`D_…`, `D01-…`/`D02-…`, `D90-…` ids; `h` headword, `s` sub-entry, `d` in a definition) |

| code             | meaning                                                      | entries |
| ---------------- | ------------------------------------------------------------ | ------: |
| `ours_wrong`     | OCR misread the page; corrected (in two rows both spellings were wrong: `note` says so, `published_form` has the scan form) |     743 |
| `his_wrong`      | the scan supports our spelling; Gheyret's list has an error  |     320 |
| `print_error`    | the printed book is wrong; corrected and listed in `print_errors.tsv` |     162 |
| `his_normalized` | Gheyret follows a spelling rule or another dictionary; the print is kept |      90 |
| `variant`        | both spellings are headwords in the 2011 or 1990s dictionary |      19 |
| `era_form`       | a deliberate older spelling; 2011 differs ("2011 form: …" in `note`) |       8 |
| `misaligned`     | the comparison paired two different entries; nothing to fix  |      15 |

**What the comparison shows.**

- Each list caught errors in the other. 55% of the differences were our OCR
  errors and 24% were errors in Gheyret's list. Two independent digitizations
  checked against the scan beat either one alone.
- `verified` means checked against the scan, not error-free. 269 of the 743
  OCR errors were in rows already marked `verified`.
- The comparison finds only places where the two lists disagree. An error both
  digitizations share is invisible to it, which is why the manual audit
  continues.
- 117 differences (0.14% of all entries) are real spelling disagreements, not
  errors. They are documented rather than forced into one form: the print is
  kept, and the other form is in `note` so that
  [uyghur-normalizer](https://github.com/UyghurReferenceTexts/uyghur-normalizer)
  can map it.

**This is not unique to Uyghur.** Every written language with a history of
spelling reform has the same categories:

| language     | two accepted spellings | kind                                                      | closest code     |
| ------------ | ---------------------- | --------------------------------------------------------- | ---------------- |
| English      | colour / color         | regional standard (Webster's dictionaries, 1806 and 1828) | `variant`        |
| English (UK) | judgement / judgment   | both accepted in one country                              | `variant`        |
| English      | connexion / connection | older spelling, now dropped                               | `era_form`       |
| German       | daß / dass             | 1996 reform; the old form was correct before              | `era_form`       |
| French       | événement / évènement  | the 1990 revision allowed the second                      | `his_normalized` |
| Dutch        | pannekoek / pannenkoek | 1995 reform                                               | `era_form`       |

## This is a snapshot

`row_id` and `image` refer to the lists as they stood at audit time. Rows the
audit later removed, or that were later excluded as Chinese transliterations,
are still listed here. All verdicts are from the first audit round
(`status_v1`). If a later round overturns any, this file will be regenerated
and the change recorded in `CHANGELOG.md`.

The scanned images themselves are not published; filenames are given so that
anyone with access to the printed volumes can find the column in question.

`gheyret_crosscheck.tsv` is frozen as of 2026-10-29 and will not be
regenerated.
