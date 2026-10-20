# Hazirqi Zaman Uyghur Tilining Izahliq Lughiti (qisqartilmisi), 2011

**ھازىرقى زامان ئۇيغۇر تىلىنىڭ ئىزاھلىق لۇغىتى (قىسقارتىلمىسى)**

Compiled by the Sh.U.A.R. Milletler Til-Yéziq Xizmiti Komitéti.
Chief compilers: Hemdulla Abdurehman Imam, Perhat Nur, Esqer Abduqadir.
Shinjang Xelq Neshriyati, Ürümchi, 2011. ISBN 978-7-228-13933-0.

First edition 1999, chief editors Abliz Yaqup and Ghenizat Gheyurani.

The abridged one-work explanatory dictionary, issued in two volumes. It follows
the 2009 orthographic standard.

<!-- stats:work -->
| | |
|---|---|
| headwords | 39,993 |
| verified against scans | 8,242 |
| excluded as Chinese transliterations | 350 |
<!-- stats:end -->

## Structure

This edition has no separate sub-entry rows; compounds are folded into the
definition text, which is not published here. The `*_subentries.tsv` files are
therefore empty, kept for schema consistency across the repository.

It does have a dedicated inflected-form column, which the other two dictionaries
lack. Those forms appear as `type=inflected` rows linked to their lemma by a
`-i1` suffix on the `row_id`.

## Audit

Both volumes went through a machine-assisted audit; see the OCR audit benchmark
in the root README. 4,195 machine-proposed corrections were verified by a native
speaker against the scans, of which 2,815 were accepted. Applying them
corrected 1,866 headwords in place, restored 941 headwords the OCR had skipped,
and removed 2 spurious rows (see `CHANGELOG.md`, release 1.1.0).

## Filtering

Entries tagged `[خەن]` (Chinese origin) in the source dictionary's own etymology
column were reviewed individually and, where confirmed as transliterations,
removed — 350 rows.

Because this edition tags etymology consistently, it also serves as the
reference used to find transliterations in the 2009 imla dictionary, which has
no etymology column of its own. See the root README and
`excluded/excluded_wordlist.tsv`.
