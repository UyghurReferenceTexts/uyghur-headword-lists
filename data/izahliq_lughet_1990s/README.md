# Uyghur Edebiy Tilining Izahliq Lughiti, vols 1–6 (1990–1999)

**ئۇيغۇر ئەدەبىي تىلىنىڭ ئىزاھلىق لۇغىتى، 1–6 توم**

Compilers: Abliz Yaqup, Ghenizat Gheyurani, Ismail Qadir, Hemdulla
Abduraxman, Perhat Nur, Abliz Emet, Esqer Abduqadir, Abduzahir Tahir,
Ablikim Réhimjan, Zayit Héwil.
Milletler Neshriyati, Beijing, 1990–1999.

| volume | year | ISBN |
|---|---|---|
| 1 | 1990 | 7-105-00928-4 |
| 2 | 1991 | 7-105-01431-8 |
| 3 | 1992 | 7-105-01692-2 |
| 4 | 1994 | 7-105-02248-5 |
| 5 | 1996 | 7-105-02578-6 |
| 6 | 1999 | 7-105-03227-8 |

The large explanatory dictionary of literary Uyghur. Six volumes, published
across a decade.

<!-- stats:work -->
| | |
|---|---|
| headwords | 52,309 |
| sub-entries | 4,833 |
| verified against scans | 1,982 |
| excluded as Chinese transliterations | 445 |
<!-- stats:end -->

Load all six volumes at once:

```python
import pandas as pd, glob
df = pd.concat(pd.read_csv(f, sep="\t", dtype=str)
               for f in sorted(glob.glob("*_headwords.tsv")))
```

## Structure

Sub-entries are their own rows in the source, flagged and linked to a parent
headword, rather than being packed into the lemma's row. The `parent_id` column
in `*_subentries.tsv` resolves that link.

Homonyms are printed with Roman numerals (`ئاتالغۇ I`, `ئاتالغۇ II`). These
collapse to a single line in the headword list, since it is an inventory of
written forms rather than of senses.

Because this is an explanatory dictionary, it records non-standard spellings in
order to correct them, printed as `X (توغرىسى: Y) ← Y`. Both forms appear:
the non-standard one as `type=nonstandard`, the correct one as
`type=corrected_form`.

## Missing pages

Scanned from copies with pages absent. Headwords on these pages are not present
in the lists:

| volume | missing pages |
|---|---|
| v1 | 215, 216, 452, 453, 689 |
| v2 | 414, 415, 450, 451, 804, 805 |
| v3 | 96, 97 |
| v4 | 6, 7, 44, 45, 80, 81, 714, 715, 716, 717, 940, 941 |
| v5 | 20, 21 |
| v6 | none |

## Filtering

Entries tagged `[خەن]` (Chinese origin) in the source dictionary's own etymology
column were reviewed individually and, where confirmed as transliterations,
removed — 439 rows across the six volumes. See the root README and
`excluded/excluded_wordlist.tsv`.
