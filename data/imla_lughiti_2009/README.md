# Hazirqi Zaman Uyghur Edebiy Tilining Imla Lughiti (2009)

**ھازىرقى زامان ئۇيغۇر ئەدەبىي تىلىنىڭ ئىملا لۇغىتى (ئىملا ۋە تەلەپپۇز قائىدىسى)**

Compilers: Mirsultan Osmanof, Tahir Abduweli, Abdughappar Abduraxman,
Memet'éli Abdurehim, Anargül Abdurehim, Enwer Exmet.
Shinjang Xelq Neshriyati, Ürümchi, 2009. ISBN 978-7-228-12817-4.

**This is the orthographic standard followed throughout the repository.** It is
the reference work that defines correct modern written Uyghur; the other two
dictionaries are checked against it.

<!-- stats:work -->
| | |
|---|---|
| headwords | 37,913 |
| sub-entries | 43,412 |
| verified against scans | 6,415 |
| excluded as Chinese transliterations | 306 |
<!-- stats:end -->

## Structure

An orthography dictionary, not an explanatory one: it gives the correct
spelling, the part of speech, and compounds, but no definitions. Sub-entries are
the compounds and multi-word expressions listed under each lemma, which is how
this dictionary records them rather than giving each its own headword.

Verb entries are printed with a stem marker; the citation form adds `-ماق`
or `-مەك`. These appear as `type=verb_stem`.

## Known source errors

Typesetting errors present in the printed edition:

- `مەخپىيلەشتۈرۈلۈش | مەخپىيلەشتۈرۈلۈش` — the first should read
  `مەخپىيلەشتۈرۈلمەك`
- `ئوبراز (ئوبزارى)` — should read `ئوبراز (ئوبرازى)`

## Audit

This volume received the most thorough machine-assisted audit of the three; see
the OCR audit benchmark in the root README. 4,045 machine-proposed corrections
were verified by a native speaker against the scans, of which 3,072 were
accepted and applied.

## Filtering

**This volume has no etymology column**, so Chinese transliterations cannot be
identified systematically the way they can in the other two dictionaries. The
300 entries removed here were found by cross-referencing forms already rejected
in the 2011 dictionary, plus individual identification during the audit.

That makes this volume's exclusion list the least complete of the three, and it
will grow as the audit proceeds. See `excluded/excluded_wordlist.tsv`.
