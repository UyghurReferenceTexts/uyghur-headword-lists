# Uyghur Headword Lists

Open, machine-readable headword lists extracted from three printed Uyghur
dictionaries, released as plain TSV under CC BY 4.0.

**129,124 written forms** and **48,249 sub-entry phrases**, each traceable to
the scanned column it came from.

[![DOI](https://zenodo.org/badge/1369704181.svg)](https://doi.org/10.5281/zenodo.22747775)
[![SWH](https://archive.softwareheritage.org/badge/origin/https://github.com/UyghurReferenceTexts/uyghur-headword-lists/)](https://archive.softwareheritage.org/browse/origin/?origin_url=https://github.com/UyghurReferenceTexts/uyghur-headword-lists)
[![ORCID](https://img.shields.io/badge/ORCID-0009--0004--4633--0408-A6CE39?logo=orcid&logoColor=white)](https://orcid.org/0009-0004-4633-0408)

---

## Why this exists

The Uyghur language is targeted for systematic eradication within its homeland of East Turkistan, which remains under brutal Chinese occupation. Since 2017, Beijing has weaponized the education system across occupied East Turkistan (imposed upon by the Chinese colonial name "Xinjiang," literally meaning "new territory") to obliterate Uyghur-language instruction and violently enforce Mandarin. Uyghurs and other Turkic peoples are routinely herded into concentration camps masquerading as "re-education" centers, where they endure psychological abuse and are forbidden from speaking their mother tongue. While the UN has formally concluded these atrocities may constitute crimes against humanity, multiple international parliaments have rightfully called this campaign exactly what it is: an ongoing genocide. What researchers clinically call "Sinicization" is, in reality, a ruthless colonial project designed to strip an entire nation of its voice, culture, and existence.

Amidst this systematic erasure, the practical urgency of this project is clear. The foundational reference works defining standard written Uyghur are now endangered physical artifacts—held in rapidly shrinking numbers and written in a script that modern digital infrastructure largely fails to support. A dictionary that exists only on paper can be easily confiscated, censored, or destroyed. A dictionary transformed into an open, versioned, and globally mirrored dataset with a permanent DOI is indestructible.

This repository provides the concrete technical foundation for that preservation: it extracts the headword inventories of three major Uyghur dictionaries and transforms them into a robust, machine-readable format. It ensures this linguistic data becomes something a machine can process, a researcher can securely cite, and a spellchecker can use to keep the language alive on modern devices.

## Why the lists are filtered

The 2011 and 1990s source dictionaries were compiled under the very same state apparatus whose assimilationist policies this project counters. One visible legacy of that state control is a layer of Chinese transliterations artificially inserted as Uyghur headwords, tagged as `[خەن]` in the original etymology columns. Standardizing a state-mandated loanword in a reference dictionary is a deliberate tactic to make it appear settled, ordinary, and official. Uyghur linguists have a specific term for the endpoint of this process: *تولۇق ئاسسىمىلياتسىيە* (total assimilation).

To counteract this, 1,108 of these tagged entries were reviewed individually across two rounds. We removed 975 from the final lists because they are forced transliterations that never took root in natural Uyghur usage — 1,082 TSV rows once their variant and sub-entry forms are counted. The remaining 133 were preserved—either because the original tag was an OCR error on a native Uyghur word, or because the loanword has been genuinely integrated into the living language.

This is a deliberate editorial intervention, and we prioritize absolute transparency over silent deletion. Every removed entry is documented in [`excluded/excluded_wordlist.tsv`](excluded/excluded_wordlist.tsv) alongside its original `row_id`, source dictionary, and scanned page, allowing anyone who disagrees with this assessment to trace or reverse the decision. Crucially, this curation was performed word-by-word by a native Uyghur speaker rather than by automated pattern matching. This human review ensures precision; for example, several rejected Chinese transliterations share spellings with legitimate Uyghur homographs (such as جازا *punishment*, پەن *science*, and باشى *his head*), which remain fully intact and protected within the dataset.

**This list will grow, and entries may be restored.** The 2009 imla dictionary has no etymology column at all, so its transliterations cannot be identified systematically — they are found by cross-reference against the 2011 dictionary, or one at a time during the ongoing audit. Expect further removals as that work proceeds. Equally, a removal is a judgment and not a verdict: where the community makes a case that a form did enter ordinary Uyghur usage, it can be restored. Open an issue.

---

## What's in the data

Each dictionary directory holds a `_headwords.tsv` and, where the source has
them, a `_subentries.tsv`.

### `*_headwords.tsv`

| column | meaning |
|---|---|
| `row_id` | stable identifier; suffixes `-i1` `-v1` `-c1` mark forms derived from a parent row |
| `entry` | the written form |
| `type` | `headword`, `verb_stem`, `variant`, `inflected`, `nonstandard`, `corrected_form` |
| `source_page` | the scanned column image this came from |
| `status_v1` | `verified`, `unverified`, or blank — see *Audit rounds* |

### `*_subentries.tsv`

| column | meaning |
|---|---|
| `sub_id` | stable identifier |
| `parent_id` | the `row_id` this phrase belongs to |
| `phrase` | the compound or multi-word entry |
| `source_page` | scanned column image |
| `status_v1` | as above |

### Entry types

- `headword` — a lemma as printed.
- `verb_stem` — printed with a stem marker; the citation form adds `-ماق`/`-مەك`.
- `inflected` — a possessive or other inflected form the dictionary prints in
  parentheses after the lemma (`ئاكسىيە (ئاكسىيەسى)` yields both).
- `variant` — an equivalent or cross-reference target (`X = Y`, `X ← Y`).
- `nonstandard` — a spelling the dictionary records in order to correct it.
- `corrected_form` — the standard spelling a `nonstandard` entry points to.

One line per distinct written form. Homonyms printed as `ئاپپاق I` and
`ئاپپاق II` collapse to a single line, since the list is an inventory of
orthography, not of senses.

---

## Coverage

| work | headwords | sub-entries | verified | excluded |
|---|---|---|---|---|
| [Imla Lughiti 2009](data/imla_lughiti_2009/) | 37,917 | 43,414 | 2,370 | 300 |
| [Izahliq Lughet 1990s](data/izahliq_lughet_1990s/) (6 vols) | 52,313 | 4,835 | 1,983 | 439 |
| [Izahliq Lughet 2011](data/izahliq_lughet_2011/) (2 vols) | 38,894 | — | 2,722 | 343 |
| **total** | **129,124** | **48,249** | **7,075** | **1,082** |

Counts are **after** the exclusions described above; the *excluded* column shows
how many rows were removed from each work.

Load a whole multi-volume work with a glob:

```python
import pandas as pd, glob
df = pd.concat(pd.read_csv(f, sep="\t", dtype=str)
               for f in glob.glob("data/izahliq_lughet_1990s/*_headwords.tsv"))
```

---

## This dataset is being corrected continuously

**These files change. That is the point, not a defect.**

The lists were produced by OCR of printed scans and then audited by a native
speaker, page by page, against the original images. The audit is ongoing and
will take months. Expect a steady stream of corrections, and expect the raw
counts above to drift as errors are fixed, duplicates merge, and skipped
entries are restored.

Pin a release tag or a DOI if you need reproducibility. Track `main` if you want
the most correct data.

### Audit rounds

`status_v1` records the first full audit pass:

- `verified` — a human compared this entry against the scan and confirmed it.
- `unverified` — not yet reached. The entry is OCR output that has passed
  automated checks only.
- *blank* — the entry was added after round 1 had already passed that page.

Currently **7,075 of 129,124 forms (5.5%) are verified**. When round 2 begins, a
`status_v2` column is appended rather than overwriting round 1, so the history
of what was checked when stays in the file. **The current status of an entry is
its last non-empty `status_vN` column.**

### Known limitations

**Missing pages.** Some source volumes were scanned from copies with pages
absent. Headwords on these pages are simply not present:

| volume | missing pages |
|---|---|
| 1990s v1 | 215, 216, 452, 453, 689 |
| 1990s v2 | 414, 415, 450, 451, 804, 805 |
| 1990s v3 | 96, 97 |
| 1990s v4 | 6, 7, 44, 45, 80, 81, 714–717, 940, 941 |
| 1990s v5 | 20, 21 |
| 1990s v6, 2011 v1, 2011 v2 | none |

**Printing errors in the sources.** The printed dictionaries contain their own
typesetting mistakes, faithfully reproduced here unless flagged. From the 2009
imla dictionary:

- `مەخپىيلەشتۈرۈلۈش | مەخپىيلەشتۈرۈلۈش` — the first should read
  `مەخپىيلەشتۈرۈلمەك`
- `ئوبراز (ئوبزارى)` — should read `ئوبراز (ئوبرازى)`

These are errors of the source, not of the extraction. Where the intended form
is unambiguous it is corrected and noted in the changelog; where it is not, the
printed form stands.

**Remaining OCR errors, and missing or spurious entries.** 94.5% of entries have
not yet been checked by a human. Assume a residual error rate in that portion.
The audit finds three distinct problems, not one: characters misread by OCR,
headwords the OCR skipped entirely, and — where a machine-assisted pass was
used — plausible-looking entries that are not in the printed dictionary at all.
All three are corrected as they are found, which is why the totals above are a
snapshot rather than a fixed figure.

---

## What machines got wrong: an OCR audit benchmark

The 2009 dictionary was audited with assistance from `gemini-3.1-pro-preview`,
which read each scanned column alongside the rows OCR had produced from it. The
result is worth publishing on its own, because it quantifies something the
field mostly guesses at: **how far a frontier model can be trusted on a
low-resource script.**

4,045 machine-proposed corrections were verified by a native speaker against the
scans.

| measure | value |
|---|---|
| accepted (real error, fix correct) | 3,072 (75.9%) |
| rejected (no error, or wrong fix) | 973 (24.1%) |

**Roughly one in four high-confidence proposals was wrong.** Self-reported
confidence carried essentially no information — 99.9% of findings claimed
"high" confidence, including a quarter that were wrong.

Accuracy split sharply by what the task actually required:

| issue type | verified | accept rate |
|---|---|---|
| `split_parenthetical` — text in the wrong column | 482 | 100% |
| `truncated_equals` — cross-reference cut off | 71 | 100% |
| `other` | 32 | 97% |
| `merged_entries` | 20 | 95% |
| `wrong_characters` — spelling | 3,042 | 80% |
| `missing_headword` — entry allegedly skipped | 395 | 7% |
| `wrong_order` | 1 | 0% |

**Reliable on layout, unreliable on language.** Where the task was positional —
*this text sits in the wrong column* — the model was essentially perfect across
553 findings. Where it required knowing what a Uyghur word should look like,
accuracy fell to 80%. Where it required understanding how a Uyghur dictionary is
organized, it collapsed to 7%: the model repeatedly reported compounds as
"missing headwords" when they were already present as sub-entries of the
preceding lemma, which is simply how these dictionaries list compounds.

A documented failure mode worth knowing about: **perseveration runs.** Once the
model adopted a false pattern it applied it mechanically down the column — in
one case 28 consecutive entries carrying the identical explanation, every one
wrong. The signature is three or more consecutive findings with verbatim
identical explanation text.

**This is the argument for the dataset.** A model cannot check Uyghur
orthography against nothing. It needs ground truth, and for Uyghur that ground
truth has existed only on paper. That is what these lists are for — including
for the models that will be asked to help correct the next dictionary.

---

## Related

- **[uyghur-normalizer](https://github.com/UyghurReferenceTexts/uyghur-normalizer)**
  — orthographic normalization to the 2009 standard.

## Sources

Full bibliographic records are in each dictionary's directory README.
Listing here is bibliographic acknowledgment only and implies no endorsement of
this project by the works' authors, editors, or publishers.

1. **ھازىرقى زامان ئۇيغۇر ئەدەبىي تىلىنىڭ ئىملا لۇغىتى** — Mirsultan Osmanof et al.
   Shinjang Xelq Neshriyati, Ürümchi, 2009. ISBN 978-7-228-12817-4.
   *The orthographic standard followed throughout this repository.*
2. **ئۇيغۇر ئەدەبىي تىلىنىڭ ئىزاھلىق لۇغىتى، 1–6 توم** — Abliz Yaqup,
   Ghenizat Gheyurani et al. Milletler Neshriyati, Beijing, 1990–1999.
3. **ھازىرقى زامان ئۇيغۇر تىلىنىڭ ئىزاھلىق لۇغىتى (قىسقارتىلمىسى)** —
   Sh.U.A.R. Milletler Til-Yéziq Xizmiti Komitéti. Shinjang Xelq Neshriyati,
   Ürümchi, 2011. ISBN 978-7-228-13933-0.

## License

[CC BY 4.0](LICENSE). You may share and adapt these lists, including
commercially, with attribution.

The license covers **this extraction** — the structural data, the
identifiers, the audit status, the organization of the lists. It does not and
cannot license the underlying dictionaries. Definitions, examples, and
explanatory text are deliberately excluded from this repository; only headwords
and sub-entry phrases are published, as a factual inventory of what words the
dictionaries record.

## Citation

See [CITATION.cff](CITATION.cff). Zenodo mints two DOIs: a **concept DOI** that
always resolves to the newest version, and a **version DOI** for each release.
The badge above points at the concept DOI so readers land on current data —
but **cite the version DOI**, since that is the one that identifies the exact
snapshot you used. Given that this dataset is corrected continuously, the
distinction matters.
