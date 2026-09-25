# Changelog

All notable changes to the published lists are recorded here.

Versions follow [semantic versioning](https://semver.org/) applied to data
rather than code:

- **MAJOR** — a breaking change to the schema. A renamed or removed column, a
  changed `row_id` scheme, a directory reorganization. Anything that breaks a
  script written against the previous release.
- **MINOR** — new material or a completed body of work. A new dictionary added,
  an audit round finished, a `status_vN` column appended, a batch of restored
  entries.
- **PATCH** — corrections within the existing schema. OCR fixes, status flips
  from `unverified` to `verified`, removed duplicates.

`row_id` values are permanent. A corrected entry keeps its id; only its `entry`
or `status_vN` changes. Ids are never reused, so an id that disappears means
that row was deleted, not renumbered.

## [Unreleased]

## [1.1.0] — 2026-09-25

The first audit pass of the 2011 dictionary is now in the published lists. Corrected headwords kept their original row ids. Rows inserted during the audit are new ids. The correction counts below are what the round-1 benchmark recorded as written to the Dictionary sheets (`benchmark 2011` in volume 1, and Part 3 of `benchmark 2011 vol2`).

Across both volumes, 4,195 proposals were checked against the scanned page and 2,815 were accepted. Written to the dictionaries: 1,866 headwords corrected in place, 941 new headword rows inserted, and 2 spurious rows deleted. Another 5 accepted proposals had already been fixed by hand, 1 rejected proposal was hand-fixed differently, and 1 accepted proposal was not applied because the row was no longer on the sheet.

The exclusion list grew from 1,082 rows to 1,101 (986 rejected entries, 133 kept). The 2009 imla list and 1990s volume 2 account for the new rejections outside the 2011 audit. The other 1990s volumes are unchanged from 1.0.0.

In the 2009 imla list, 4,045 headwords and 8,074 sub-entries were marked verified. Of the published headwords, 16,561 of 130,218 (12.7%) are now verified.

### Added

- OCR audit benchmark for the 2011 dictionary: 4,195 machine-proposed
  corrections verified by a native speaker across both volumes (2,815 accepted,
  67.1%), with accept rates by error type, a vol 1 / vol 2 replication check,
  and a direction-by-direction analysis of vowel substitutions. The README
  benchmark section now covers both the 2009 and 2011 dictionaries.

### Changed

- `imla_lughiti_2009` — 4 headwords and 2 sub-entries removed as Chinese transliterations; 4,045 headwords and 8,074 sub-entries newly verified.
- `izahliq_lughet_1990s_v2` — 4 headwords and 2 sub-entries removed as Chinese transliterations.
- `izahliq_lughet_2011_v1` — 2,194 proposals audited, 1,397 accepted. Written to the sheet: 906 headwords corrected in place, 487 new headword rows inserted, 2 spurious rows deleted. Two further proposals had already been fixed by hand, and one rejected proposal was hand-fixed differently.
- `izahliq_lughet_2011_v2` — 2,001 proposals audited, 1,418 accepted. Written to the sheet: 960 headwords corrected in place, 454 new headword rows inserted. Three further proposals had already been fixed by hand, and one accepted proposal was not applied because the row was gone. The last 4 open findings were rejected, so the volume 2 queue is complete and nothing further was written.

## [1.0.0] — 2026-09-14

First public release.

### Added

- 129,124 written forms and 48,249 sub-entry phrases from three dictionaries:
  - `imla_lughiti_2009` — 37,917 headwords, 43,414 sub-entries
  - `izahliq_lughet_1990s` — 52,313 headwords, 4,835 sub-entries (6 volumes)
  - `izahliq_lughet_2011` — 38,894 headwords (2 volumes)
- `status_v1` column recording the first audit round. 7,075 forms (5.5%)
  verified against the original scans at time of release.
- `excluded/excluded_wordlist.tsv` — 1,082 rows removed as Chinese
  transliterations (975 entries reviewed and rejected across two rounds, out of
  1,108 reviewed), published in full with their row_ids so the decision is
  reversible.
- OCR audit benchmark for the 2009 dictionary: 4,045 machine-proposed
  corrections verified by a native speaker, with accept rates by error type.

### Known issues

- 94.5% of entries are `unverified` and carry a residual OCR error rate.
- Several source volumes were scanned from copies with missing pages; see the
  table in `README.md`.
- Printing errors present in the source dictionaries are reproduced unless the
  intended form is unambiguous.
- The exclusion list is incomplete for the 2009 imla dictionary, which has no
  etymology column; entries there are found by cross-reference and during the
  audit, so `excluded/` will grow in future releases.
