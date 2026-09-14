# Publishing checklist

Internal notes. Not part of the dataset.

## Before the first push

1. **Rebuild and filter.** The TSVs must be regenerated and re-filtered every
   time, in this order:

   ```
   python build_wordlist.py . --out export
   python apply_exclusions.py export han_origin_review.xlsx
   python apply_exclusions.py export han_origin_review_round2.xlsx
   ```

   `build_wordlist.py` writes the full list each run, so skipping step 2 or 3
   puts the Chinese transliterations back. Check the printed count: it should
   report finding roughly 680 and 292 rejected ids. A number far below that
   means it ran against already-filtered files.

2. **Sort every TSV by `row_id`** before committing. Unsorted files produce
   diffs that are impossible to read, and the whole value of the changelog
   depends on a one-line-per-correction diff.

3. **Publish the exclusion lists.** Copy the two review workbooks' rejected rows
   into `excluded/` as TSV. The README promises this and it is what makes the
   filtering decision auditable rather than editorial fiat.

4. **Check file sizes.** Anything over 50 MB triggers a GitHub warning, over
   100 MB is rejected. The largest list here is well under both.

5. **Verify no `.xlsx` is staged.** The spreadsheets contain definitions and
   must never be committed. Add to `.gitignore`:

   ```
   *.xlsx
   *.xlsx.bak
   *.jsonl
   *_review.tsv
   ```

## GitHub

1. Create `uyghur-headword-lists` under the `UyghurReferenceTexts`
   organization. Public. **Do not** let GitHub add a README, .gitignore, or
   license — you have your own.

2. Add the license through the GitHub web UI rather than pasting the text:
   *Add file → Create new file → type `LICENSE` → "Choose a license template"
   → Creative Commons Attribution 4.0*. This inserts the official legal code
   with no transcription risk, which matters for a legal document.

3. Push everything else.

4. Repository settings: add the description, the topics `uyghur`, `lexicon`,
   `dictionary`, `low-resource-nlp`, `ocr`, and link to the normalizer repo.

## Zenodo

**Order matters. Enable the repository in Zenodo *before* creating the
release** — Zenodo only archives releases published after the switch is on. If
you tag first, the release is invisible to Zenodo and you have to cut another.

1. Sign in to [zenodo.org](https://zenodo.org) with GitHub.
2. Get an ORCID first if you don't have one — [orcid.org](https://orcid.org),
   free, five minutes. It permanently disambiguates you as the author and is
   worth having before the first DOI rather than after.
3. Zenodo → your profile → **GitHub** → flip the switch on
   `UyghurReferenceTexts/uyghur-headword-lists`.
4. Fill in the real values in `CITATION.cff` and `.zenodo.json` — name, ORCID,
   date. Commit. `.zenodo.json` is what Zenodo actually reads; without it
   Zenodo guesses from the repo and usually guesses badly.
5. On GitHub: **Releases → Create a new release**.
   - Tag: `v1.0.0`
   - Title: `v1.0.0 — first public release`
   - Body: paste the `1.0.0` section of `CHANGELOG.md`
   - Publish.
6. Zenodo picks it up within a few minutes and mints a DOI.

### Getting the version to read 1.0.0

Zenodo takes its version string from the git tag. Tag `v1.0.0` and the record
reads `v1.0.0`; setting `"version": "1.0.0"` in `.zenodo.json` makes it read
exactly `1.0.0`. Either is fine — just be consistent from the first release,
because it is the thing every later citation will be compared against.

There is no requirement that a dataset be "finished" to be 1.0.0. 1.0.0 means
the schema is stable and the thing is usable, which is true. An incomplete audit
is a documented limitation, not a reason to hide behind 0.x — and 0.x versions
get cited less and indexed worse.

### Two DOIs, and which to use

Zenodo mints **two**:

- a **concept DOI** that always resolves to the newest version
- a **version DOI** unique to `v1.0.0`

Put the concept DOI badge in the README so readers land on current data. Tell
researchers to cite the **version DOI**, because that is the one that is
reproducible. Say this explicitly in the README citation section — most people
don't know the difference and will cite whichever you show them.

### Release cadence

Every GitHub release mints a new DOI, so don't release on every correction.
Commit corrections to `main` continuously; cut a Zenodo release quarterly, or
whenever a volume's audit round completes. `main` is the living dataset,
releases are the citable snapshots.

## Hugging Face

Worth doing, as a **mirror, not a second source of truth**. GitHub stays
canonical; everything else is generated from it. Two sources of truth for the
same data will diverge, and the one people find first will be the stale one.

1. Create `UyghurReferenceTexts/uyghur-headword-lists` as a **dataset** repo.
2. Ship `.parquet` alongside the TSVs so `load_dataset` works with no config.
   Keep the TSVs too — they are the human-readable form and they diff.
3. The dataset card is a copy of the README with YAML frontmatter on top:

   ```yaml
   ---
   language: [ug]
   license: cc-by-4.0
   task_categories: [token-classification, text-generation]
   size_categories: [100K<n<1M]
   tags: [uyghur, lexicon, dictionary, orthography, ocr, low-resource]
   ---
   ```

   `size_categories` is `100K<n<1M` — 130,078 rows, not the `10K<n<100K` in the
   earlier draft.

4. Link back to the GitHub repo and the Zenodo DOI in the card, and say plainly
   that GitHub is canonical.

## After publishing

- Add the Zenodo badge to the README (the commented-out line at the top).
- Cross-link from the normalizer repo.
- Announce where Uyghur NLP and digital-humanities people actually are.

## What not to do yet

The roadmap's Wikidata phase — registering 130,000 headwords as Lexemes —
should wait. A mass import of that size without prior discussion on the
Wikidata project chat tends to get reverted, and a reverted import is much more
work than a discussed one. Raise it there first, start with a few hundred
verified entries, and expand once the modelling is agreed. Same for the
Hunspell build: worth doing, but it wants the verified fraction to be well above
5% before it is genuinely useful as a spellchecker.
