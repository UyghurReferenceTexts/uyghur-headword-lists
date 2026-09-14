# Excluded entries

`excluded_wordlist.tsv` lists every row removed from the published headword
lists as a Chinese transliteration. It exists so the filtering decision is
auditable rather than invisible — see *Why the lists are filtered* in the root
README for the reasoning.

**1,082 rows**, from 975 entries rejected across two review rounds.

| column | meaning |
|---|---|
| `dictionary` | which work and volume the row came from |
| `row_id` | the identifier it had in the published list |
| `entry` | the written form |
| `type` | `headword`, `variant`, `inflected`, etc. |
| `source_page` | the scanned column image |
| `status_v1` | audit status at time of removal |
| `parent_id` | for sub-entry rows, the headword they belonged to |

| dictionary | rows removed |
|---|---:|
| Imla 2009 | 300 |
| 1990s v1 | 52 |
| 1990s v2 | 126 |
| 1990s v3 | 123 |
| 1990s v4 | 80 |
| 1990s v5 | 24 |
| 1990s v6 | 34 |
| 2011 v1 | 242 |
| 2011 v2 | 101 |
| **total** | **1,082** |

## This list is not final

It will grow. The 2009 imla dictionary has no etymology column, so its
transliterations cannot be found systematically — only by cross-reference
against the 2011 dictionary or one at a time during the ongoing audit. Its 300
removals are the least complete of the three works.

Entries can also move the other way. A removal is a judgment about whether a
form entered ordinary Uyghur usage, not a permanent verdict. If you can make
the case that one of these belongs in the lists, open an issue and it will be
reconsidered.

Restoring a row is exact rather than approximate: `row_id` values are permanent
and never reused, so any row here can be put back precisely where it was.
