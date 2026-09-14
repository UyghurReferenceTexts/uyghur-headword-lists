# Changelog
# doi: "10.5281/zenodo.22747775"

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
