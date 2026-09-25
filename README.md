# Uyghur Headword Lists

Open, machine-readable headword lists extracted from three printed Uyghur
dictionaries, released as plain TSV under CC BY 4.0.

<!-- stats:headline -->
**130,218 written forms** and **48,245 sub-entry phrases**, each traceable to
the scanned column it came from.
<!-- stats:end -->

[![DOI](https://zenodo.org/badge/1369704181.svg)](https://doi.org/10.5281/zenodo.22747775)
[![SWH](https://archive.softwareheritage.org/badge/origin/https://github.com/UyghurReferenceTexts/uyghur-headword-lists/)](https://archive.softwareheritage.org/browse/origin/?origin_url=https://github.com/UyghurReferenceTexts/uyghur-headword-lists)
[![ORCID](https://img.shields.io/badge/ORCID-0009--0004--4633--0408-A6CE39?logo=orcid&logoColor=white)](https://orcid.org/0009-0004-4633-0408)
[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)

---

## Why this exists

The Uyghur language is targeted for systematic eradication within its homeland of East Turkistan, which remains under brutal Chinese occupation. Since 2017, Beijing has weaponized the education system across occupied East Turkistan (imposed upon by the Chinese colonial name "Xinjiang," literally meaning "new territory") to obliterate Uyghur-language instruction and violently enforce Mandarin. Uyghurs and other Turkic peoples are routinely herded into concentration camps masquerading as "re-education" centers, where they endure psychological abuse and are forbidden from speaking their mother tongue. While the UN has formally concluded these atrocities may constitute crimes against humanity, multiple international parliaments have rightfully called this campaign exactly what it is: an ongoing genocide. What researchers clinically call "Sinicization" is, in reality, a ruthless colonial project designed to strip an entire nation of its voice, culture, and existence.

Amidst this systematic erasure, the practical urgency of this project is clear. The foundational reference works defining standard written Uyghur are now endangered physical artifacts—held in rapidly shrinking numbers and written in a script that modern digital infrastructure largely fails to support. A dictionary that exists only on paper can be easily confiscated, censored, or destroyed. A dictionary transformed into an open, versioned, and globally mirrored dataset with a permanent DOI is indestructible.

This repository provides the concrete technical foundation for that preservation: it extracts the headword inventories of three major Uyghur dictionaries and transforms them into a robust, machine-readable format. It ensures this linguistic data becomes something a machine can process, a researcher can securely cite, and a spellchecker can use to keep the language alive on modern devices.

## Why the lists are filtered

The 2011 and 1990s source dictionaries were compiled under the very same state apparatus whose assimilationist policies this project counters. One visible legacy of that state control is a layer of Chinese transliterations artificially inserted as Uyghur headwords, tagged as `[خەن]` in the original etymology columns. Standardizing a state-mandated loanword in a reference dictionary is a deliberate tactic to make it appear settled, ordinary, and official. Uyghur linguists have a specific term for the endpoint of this process: *تولۇق ئاسسىمىلياتسىيە* (total assimilation).

To counteract this, 1,119 entries were reviewed individually across two rounds. We removed 986 from the final lists because they are forced transliterations that never took root in natural Uyghur usage — 1,101 TSV rows once their variant and sub-entry forms are counted. The remaining 133 were preserved—either because the original tag was an OCR error on a native Uyghur word, or because the loanword has been genuinely integrated into the living language.

This is a deliberate editorial intervention, and we prioritize absolute transparency over silent deletion. Every removed entry is documented in [`excluded/excluded_wordlist.tsv`](excluded/excluded_wordlist.tsv) alongside its original `row_id`, source dictionary, and scanned page, allowing anyone who disagrees with this assessment to trace or reverse the decision. Crucially, this curation was performed word-by-word by a native Uyghur speaker rather than by automated pattern matching. This human review ensures precision; for example, several rejected Chinese transliterations share spellings with legitimate Uyghur homographs (such as جازا *punishment*, پەن *science*, and باشى *his head*), which remain fully intact and protected within the dataset.

**This list will grow, and entries may be restored.** The 2009 imla dictionary has no etymology column at all, so its transliterations cannot be identified systematically — they are found by cross-reference against the 2011 dictionary, or one at a time during the ongoing audit. Expect further removals as that work proceeds. Equally, a removal is a judgment and not a verdict: where the community makes a case that a form did enter ordinary Uyghur usage, it can be restored. Open an issue.

---

## What's in the data

Each dictionary directory holds a `_headwords.tsv` and, where the source has
them, a `_subentries.tsv`.

### `*_headwords.tsv`

| column        | meaning                                                      |
| ------------- | ------------------------------------------------------------ |
| `row_id`      | stable identifier; suffixes `-i1` `-v1` `-c1` mark forms derived from a parent row |
| `entry`       | the written form                                             |
| `type`        | `headword`, `verb_stem`, `variant`, `inflected`, `nonstandard`, `corrected_form` |
| `source_page` | the scanned column image this came from                      |
| `status_v1`   | `verified`, `unverified`, or blank — see *Audit rounds*      |

### `*_subentries.tsv`

| column        | meaning                             |
| ------------- | ----------------------------------- |
| `sub_id`      | stable identifier                   |
| `parent_id`   | the `row_id` this phrase belongs to |
| `phrase`      | the compound or multi-word entry    |
| `source_page` | scanned column image                |
| `status_v1`   | as above                            |

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

<!-- stats:coverage -->

| work                                                        | headwords   | sub-entries | verified   | excluded  |
| ----------------------------------------------------------- | ----------- | ----------- | ---------- | --------- |
| [Imla Lughiti 2009](data/imla_lughiti_2009/)                | 37,913      | 43,412      | 6,415      | 306       |
| [Izahliq Lughet 1990s](data/izahliq_lughet_1990s/) (6 vols) | 52,309      | 4,833       | 1,982      | 445       |
| [Izahliq Lughet 2011](data/izahliq_lughet_2011/) (2 vols)   | 39,996      | 0           | 8,164      | 350       |
| **total**                                                   | **130,218** | **48,245**  | **16,561** | **1,101** |
| <!-- stats:end -->                                          |             |             |            |           |

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

<!-- stats:verified -->
Currently **16,561 of 130,218 forms (12.7%) are verified**.
<!-- stats:end -->

 When round 2 begins, a
`status_v2` column is appended rather than overwriting round 1, so the history
of what was checked when stays in the file. **The current status of an entry is
its last non-empty `status_vN` column.**

### Known limitations

**Missing pages.** Some source volumes were scanned from copies with pages
absent. Headwords on these pages are simply not present:

| volume                     | missing pages                           |
| -------------------------- | --------------------------------------- |
| 1990s v1                   | 215, 216, 452, 453, 689                 |
| 1990s v2                   | 414, 415, 450, 451, 804, 805            |
| 1990s v3                   | 96, 97                                  |
| 1990s v4                   | 6, 7, 44, 45, 80, 81, 714–717, 940, 941 |
| 1990s v5                   | 20, 21                                  |
| 1990s v6, 2011 v1, 2011 v2 | none                                    |

**Printing errors in the sources.** The printed dictionaries contain their own
typesetting mistakes, faithfully reproduced here unless flagged. From the 2009
imla dictionary:

- `مەخپىيلەشتۈرۈلۈش | مەخپىيلەشتۈرۈلۈش` — the first should read
  `مەخپىيلەشتۈرۈلمەك`
- `ئوبراز (ئوبزارى)` — should read `ئوبراز (ئوبرازى)`

These are errors of the source, not of the extraction. Where the intended form
is unambiguous it is corrected and noted in the changelog; where it is not, the
printed form stands.

**Remaining OCR errors, and missing or spurious entries.** 87.3% of entries have
not yet been checked by a human. Assume a residual error rate in that portion.
The audit finds three distinct problems, not one: characters misread by OCR,
headwords the OCR skipped entirely, and — where a machine-assisted pass was
used — plausible-looking entries that are not in the printed dictionary at all.
All three are corrected as they are found, which is why the totals above are a
snapshot rather than a fixed figure.

---

## What machines got wrong: an OCR audit benchmark

Two of the dictionaries were audited with assistance from `gemini-3.1-pro-preview`,
which read each scanned column alongside the rows OCR had produced from it. Every
proposed correction was then checked by a native speaker against the scan. The
result is worth publishing on its own, because it quantifies something the field
mostly guesses at: **how far a frontier model can be trusted on a low-resource
script.**

| dictionary                 | proposals verified | accepted  | precision |
| -------------------------- | ------------------ | --------- | --------- |
| Imla Lughiti 2009          | 4,045              | 3,072     | 75.9%     |
| Izahliq Lughet 2011, vol 1 | 2,194              | 1,397     | 63.7%     |
| Izahliq Lughet 2011, vol 2 | 2,001              | 1,418     | 70.9%     |
| **total**                  | **8,240**          | **5,887** | **71.4%** |

**Between one in four and one in three proposals was wrong, and the model never
said so.** Self-reported confidence carried no information: 99.9% of the 2009
findings and 4,194 of the 4,195 findings in 2011 were labelled "high",
including every one that was wrong. There is no threshold that separates right
from wrong.

### Imla Lughiti 2009

| issue type                                       | verified | accept rate |
| ------------------------------------------------ | -------- | ----------- |
| `split_parenthetical` — text in the wrong column | 482      | 100%        |
| `truncated_equals` — cross-reference cut off     | 71       | 100%        |
| `other`                                          | 32       | 97%         |
| `merged_entries`                                 | 20       | 95%         |
| `wrong_characters` — spelling                    | 3,042    | 80%         |
| `missing_headword` — entry allegedly skipped     | 395      | 7%          |
| `wrong_order`                                    | 1        | 0%          |

**Reliable on layout, unreliable on language.** Where the task was positional —
*this text sits in the wrong column* — the model was essentially perfect across
553 findings. Where it required knowing what a Uyghur word should look like,
accuracy fell to 80%. Where it required understanding how a Uyghur dictionary is
organized, it collapsed to 7%: the model repeatedly reported compounds as
"missing headwords" when they were already present as sub-entries of the
preceding lemma, which is simply how this dictionary lists compounds.

### Izahliq Lughet 2011

The two volumes were audited independently.

| issue type                                     | vol 1     | vol 2     | combined          |
| ---------------------------------------------- | --------- | --------- | ----------------- |
| `truncated_arrow_equals` — cross-reference cut | 139/139   | 112/112   | 251/251 (100%)    |
| `missing_headword` — entry skipped by OCR      | 487/544   | 454/479   | 941/1,023 (92%)   |
| `wrong_characters` — spelling                  | 760/1,324 | 846/1,308 | 1,606/2,632 (61%) |
| `inflection_error`                             | 8/127     | 5/60      | 13/187 (7%)       |
| `extra_row` — entry allegedly spurious         | 2/47      | 0/38      | 2/85 (2%)         |
| `wrong_order`                                  | 0/11      | 0/3       | 0/14 (0%)         |
| `other`, `merged_entries`                      | 1/2       | 1/1       | 2/3               |

The volume 2 figure is higher overall mainly because its mix is heavier in
`missing_headword`, a class the model does well; class by class, the volumes
agree.

**Vowels are where it fails, and it fails in one direction.** Within
`wrong_characters`, proposals whose only change was a vowel letter
(ا ە ى و ۇ ۆ ۈ ې) were right just 36.6% of the time — 36.2% in volume 1, 37.0% in
volume 2, 774 proposals in all. Split by direction, the same vowel pair behaves
completely differently:

| OCR read → model proposed | correct | precision | reverse direction | correct | precision |
| ------------------------- | ------- | --------- | ----------------- | ------- | --------- |
| ۇ → و                     | 7/116   | 6%        | و → ۇ             | 39/43   | 91%       |
| ۈ → ۆ                     | 1/71    | 1%        | ۆ → ۈ             | 10/11   | 91%       |
| ۇ → ۆ                     | 7/84    | 8%        | ۆ → ۇ             | 3/3     | —         |
| ى → ا                     | 30/31   | 97%       | ا → ى             | 19/22   | 86%       |
| ە → ى                     | 36/44   | 82%       | ى → ە             | 23/45   | 51%       |
| ې → ى                     | 8/45    | 18%       | ى → ې             | 4/24    | 17%       |

When the model proposes replacing ۇ or ۈ with a rounded vowel it prefers,
the OCR was right about 94% of the time: the proposal is an anti-signal. When
it restores a vowel the OCR dropped, it is usually right. It is not reading the
rounded-vowel contrasts off the page; it is substituting what its training
data made frequent. The effect is the same size in two independently audited
volumes, which makes it a property of the model rather than of either book —
and a cheap discriminator for future models: the ۇ/و and ۈ/ۆ proposals separate
a model that reads Uyghur script from one that predicts it.

### Across both dictionaries

- **Positional work is solved.** Cut-off cross-references were 322 of 322
  correct across both books.
- **The same class can flip with the book.** `missing_headword` was 7% in 2009
  and 92% in 2011. The model's skill at spotting a dropped line did not change;
  what changed is whether the dictionary nests compounds under the preceding
  lemma. Claims that depend on a dictionary's conventions have to be judged per
  dictionary.
- **Perseveration runs.** Once the model adopts a false pattern it applies it
  mechanically. In 2009, one run of 28 consecutive findings carried the
  identical explanation, every one wrong; the signature is three or more
  consecutive findings with verbatim identical explanation text. In 2011 the
  same behaviour appeared as whole word families re-spelled in one sweep
  (دىڭگۇك 9 entries, زاسۈي 6, پەرمۇدە 4): every proposal rejected.
- **Judgement classes are noise.** `extra_row`, `wrong_order` and
  `inflection_error` together were right 15 times in 286 — the model reads
  legitimate continuation lines, sub-entry order and recorded inflections as
  errors.

All figures are from the first audit round (`status_v1`). A small number of
verdicts may be overturned in round 2, and the figures will be re-derived then.

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
