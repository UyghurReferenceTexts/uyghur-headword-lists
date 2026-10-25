# Changelog

All notable changes to the published lists are recorded here.

Versions follow [semantic versioning](https://semver.org/) applied to data
rather than code:

* **MAJOR** — a breaking change to the schema. A renamed or removed column, a
changed `row_id` scheme, a directory reorganization. Anything that breaks a
script written against the previous release.
* **MINOR** — new material or a completed body of work. A new dictionary added,
an audit round finished, a `status_vN` column appended, a batch of restored
entries.
* **PATCH** — corrections within the existing schema. OCR fixes, status flips
from `unverified` to `verified`, removed duplicates.

`row_id` values are permanent. A corrected entry keeps its id; only its `entry`
or `status_vN` changes. Ids are never reused, so an id that disappears means
that row was deleted, not renumbered.

## [Unreleased]

## [1.3.3] — 2026-10-25

327 entries corrected, 23 headwords added, 6 removed, 13,004 newly verified.

### Changed
- `imla_lughiti_2009` — 326 entries corrected, 23 headwords added, 6 removed, 9 sub-entries added, 46 sub-entries removed, 12,989 newly verified.
- `izahliq_lughet_1990s_v4` — 1 entries corrected, 8 newly verified.
- `izahliq_lughet_2011_v1` — 6 newly verified.
- `izahliq_lughet_2011_v2` — 1 newly verified.

### Corrected by hand
* `imla_lughiti_2009` — 260 OCR misspellings corrected by hand (106 headwords, 154 sub-entries); 20 headword rows inserted; 17 sub-entries moved out of a sub-entry cell (mostly into their own headword rows); 30 entries removed; 1 sub-entries added; 3 POS labels changed; 6,917 headwords and 6,158 sub-entries newly verified.

## [1.3.2] — 2026-10-20

175 entries corrected, 11 headwords added, 10 removed, 8,636 newly verified.

The manual audit of the 2009 imla dictionary now reaches `D_10053` تۈرمەللەن _ (source column 219_col2). In 1.3.1 it had reached `D_05539` پارتاڭچى (source column 119_col3).

### Changed
- `imla_lughiti_2009` — 168 entries corrected, 10 headwords added, 2 removed, 8 sub-entries added, 15 sub-entries removed, 8,369 newly verified.
- `izahliq_lughet_1990s_v1` — 1 removed, 16 newly verified.
- `izahliq_lughet_1990s_v2` — 1 entry corrected, 1 removed, 1 newly verified.
- `izahliq_lughet_1990s_v3` — 2 entries corrected, 4 removed, 60 newly verified.
- `izahliq_lughet_1990s_v4` — 1 entry corrected, 1 removed, 23 newly verified.
- `izahliq_lughet_1990s_v5` — 1 headword added, 1 removed, 80 newly verified.
- `izahliq_lughet_1990s_v6` — 11 newly verified.
- `izahliq_lughet_2011_v1` — 2 entries corrected, 57 newly verified.
- `izahliq_lughet_2011_v2` — 1 entry corrected, 19 newly verified.

### Corrected by hand
* `imla_lughiti_2009` — 160 OCR misspellings corrected by hand (43 headwords, 117 sub-entries); 7 misprints of the printed book corrected (listed in `audit/print_errors.tsv`); 5 headword rows inserted; 16 sub-entries moved out of a sub-entry cell (mostly into their own headword rows); 1 entry removed; 6 sub-entries added; 4,040 headwords and 4,347 sub-entries newly verified.

## [1.3.1] — 2026-10-16

499 entries corrected, 26 headwords added, 15 removed, 7,178 newly verified.

### Changed
- `imla_lughiti_2009` — 473 entries corrected, 26 headwords added, 13 removed, 10 sub-entries added, 24 sub-entries removed, 7,159 newly verified.
- `izahliq_lughet_1990s_v1` — 19 newly verified.
- `izahliq_lughet_2011_v2` — 26 entries corrected, 2 removed.

## [1.3.0] — 2026-10-13

### Added

* `audit/gemini_audit.tsv` — the full record behind the OCR audit benchmark:
every correction proposed during the audits of all three dictionaries (16,149
rows), with the native speaker's verdict, the model's explanation and
confidence, and who proposed it. Filtering to Gemini's accepted and rejected
proposals reproduces the benchmark figures in the README (15,934 proposals,
9,560 accepted).
* `audit/print_errors.tsv` — 48 misprints in the printed books (3 in the 2009
dictionary, 42 in the 1990s volumes 1 and 3–6, and 3 in 2011 volume 2), each
with the printed form, the corrected form used in the lists, and the scanned
column.
* `export_audit.py`, which generates both files from the dictionary workbooks.

## [1.2.0] — 2026-10-12

The first full headword audit of the six 1990s volumes is now in the published lists. Corrected entries kept their original row ids. Rows inserted during the audit are new ids. Compared with 1.1.0, those volumes gained 1,829 headwords and lost 339, and 1,719 existing entries had their spelling corrected. Sub-entries went from 4,833 to 4,829. The 2009 imla lists are unchanged. In the 2011 lists, three volume-2 headwords were marked verified. Volume 1 corrected `D01-11385` from جاك Ⅰ to جاك, and the duplicate headword `D01-11386` collapsed into it.

Of the published headwords, 25,211 of 131,707 (19.1%) are now verified. The 7,506 newly verified rows in the volume lines below already existed in 1.1.0 and flipped to verified; that figure counts sub-entries as well as headwords. Headwords added by the audit and already marked verified are included in the 25,211, not in the 7,506.

The exclusion list went from 1,101 rows to 1,099 (982 rejected entries, 138 kept). `D90-2-03080` تۇخاۋ-لېيشېن is a new rejection. Three further rows are variant forms of entries that were already rejected. جا and پاڭ, with their sub-entries, were kept and are back in the published lists. `D90-3-08063` (شۇ ← شۇ خانلىقى) no longer has its own row: the written form شۇ is the earlier verified headword `D90-3-08041`, so the duplicate was collapsed rather than deleted.

### Added

* OCR audit benchmark for the 1990s dictionary: 7,694 machine-proposed
corrections verified by a native speaker across all six volumes (3,673
accepted, 47.7%). The README benchmark section now covers all three
dictionaries, with a per-volume summary, the four edit patterns behind most
false corrections, and a side-by-side comparison of vowel substitutions in the
1990s and 2011 dictionaries.

### Changed

* `izahliq_lughet_1990s_v1` — 232 entries corrected, 301 headwords added, 51 removed, 13 sub-entries added, 4 sub-entries removed, 1,294 newly verified.
* `izahliq_lughet_1990s_v2` — 273 entries corrected, 322 headwords added, 47 removed, 3 sub-entries added, 4 sub-entries removed, 1,158 newly verified.
* `izahliq_lughet_1990s_v3` — 292 entries corrected, 309 headwords added, 58 removed, 1 sub-entry removed, 1,405 newly verified.
* `izahliq_lughet_1990s_v4` — 370 entries corrected, 326 headwords added, 96 removed, 3 sub-entries removed, 1,325 newly verified.
* `izahliq_lughet_1990s_v5` — 255 entries corrected, 305 headwords added, 47 removed, 8 sub-entries removed, 1,233 newly verified.
* `izahliq_lughet_1990s_v6` — 297 entries corrected, 266 headwords added, 40 removed, 1 sub-entry added, 1 sub-entry removed, 1,088 newly verified.
* `izahliq_lughet_2011_v1` — 1 entry corrected, 1 removed.
* `izahliq_lughet_2011_v2` — 3 headwords newly verified.

## [1.1.0] — 2026-09-25

The first audit pass of the 2011 dictionary is now in the published lists. Corrected headwords kept their original row ids. Rows inserted during the audit are new ids. The correction counts below are what the round-1 benchmark recorded as written to the Dictionary sheets (`benchmark 2011` in volume 1, and Part 3 of `benchmark 2011 vol2`).

Across both volumes, 4,195 proposals were checked against the scanned page and 2,815 were accepted. Written to the dictionaries: 1,866 headwords corrected in place, 941 new headword rows inserted, and 2 spurious rows deleted. Another 5 accepted proposals had already been fixed by hand, 1 rejected proposal was hand-fixed differently, and 1 accepted proposal was not applied because the row was no longer on the sheet.

The exclusion list grew from 1,082 rows to 1,101 (986 rejected entries, 133 kept). The 2009 imla list and 1990s volume 2 account for the new rejections outside the 2011 audit. The other 1990s volumes are unchanged from 1.0.0.

In the 2009 imla list, 4,045 headwords and 8,074 sub-entries were marked verified. Of the published headwords, 16,561 of 130,218 (12.7%) are now verified.

### Added

* OCR audit benchmark for the 2011 dictionary: 4,195 machine-proposed
corrections verified by a native speaker across both volumes (2,815 accepted,
67.1%), with accept rates by error type, a vol 1 / vol 2 replication check,
and a direction-by-direction analysis of vowel substitutions. The README
benchmark section now covers both the 2009 and 2011 dictionaries.

### Changed

* `imla_lughiti_2009` — 4 headwords and 2 sub-entries removed as Chinese transliterations; 4,045 headwords and 8,074 sub-entries newly verified.
* `izahliq_lughet_1990s_v2` — 4 headwords and 2 sub-entries removed as Chinese transliterations.
* `izahliq_lughet_2011_v1` — 2,194 proposals audited, 1,397 accepted. Written to the sheet: 906 headwords corrected in place, 487 new headword rows inserted, 2 spurious rows deleted. Two further proposals had already been fixed by hand, and one rejected proposal was hand-fixed differently.
* `izahliq_lughet_2011_v2` — 2,001 proposals audited, 1,418 accepted. Written to the sheet: 960 headwords corrected in place, 454 new headword rows inserted. Three further proposals had already been fixed by hand, and one accepted proposal was not applied because the row was gone. The last 4 open findings were rejected, so the volume 2 queue is complete and nothing further was written.

## [1.0.0] — 2026-09-14

First public release.

### Added

* 129,124 written forms and 48,249 sub-entry phrases from three dictionaries:

  * `imla_lughiti_2009` — 37,917 headwords, 43,414 sub-entries
  * `izahliq_lughet_1990s` — 52,313 headwords, 4,835 sub-entries (6 volumes)
  * `izahliq_lughet_2011` — 38,894 headwords (2 volumes)
* `status_v1` column recording the first audit round. 7,075 forms (5.5%)
verified against the original scans at time of release.
* `excluded/excluded_wordlist.tsv` — 1,082 rows removed as Chinese
transliterations (975 entries reviewed and rejected across two rounds, out of
1,108 reviewed), published in full with their row_ids so the decision is
reversible.
* OCR audit benchmark for the 2009 dictionary: 4,045 machine-proposed
corrections verified by a native speaker, with accept rates by error type.

### Known issues

* 94.5% of entries are `unverified` and carry a residual OCR error rate.
* Several source volumes were scanned from copies with missing pages; see the
table in `README.md`.
* Printing errors present in the source dictionaries are reproduced unless the
intended form is unambiguous.
* The exclusion list is incomplete for the 2009 imla dictionary, which has no
etymology column; entries there are found by cross-reference and during the
audit, so `excluded/` will grow in future releases.

