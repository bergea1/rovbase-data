# Rovbase — carnivore records for mapping

Norwegian and Swedish large-carnivore records (lynx, wolverine, bear, wolf, golden eagle),
enriched with the damage-assessment text that [rovbase.no](https://www.rovbase.no) shows in its
map tooltip, and shaped for mapping tools.

**Updated daily, automatically.** Rebuilt whenever [Rovbase's GBIF
dataset](https://ipt.gbif.no/resource?r=rovbase) publishes a new version. Each commit is tagged
with the GBIF version it came from (`gbif-v1.298`).

## Files

| File | Records | For |
|---|---|---|
| `rovbase-no-1y.csv` | ~1,700 | Norway, last 12 months — **best for Datawrapper** |
| `rovbase-no-5y.csv` | ~10,600 | Norway, last 5 years |
| `rovbase-no.csv` | ~74,600 | Norway, all time |
| `rovbase-1y.csv` / `-5y.csv` | ~2,600 / ~16,600 | All countries, windowed |
| `rovbase.csv` | ~90,100 | Everything |
| `*.geojson` | — | Same data as GeoJSON (EPSG:4326) for QGIS / Mapshaper / Felt |
| `versions.jsonl` | — | One line per upstream publish: date, GBIF version, row count, checksum |

Time windows are rolling from the day the file was cut, not calendar years.

## Using it in Datawrapper

Datawrapper **symbol maps take CSV with lat/lon columns and do not accept GeoJSON** (GeoJSON is
only for locator maps, which cap at 500 markers). So point it at a CSV:

```
https://raw.githubusercontent.com/bergea1/rovbase-data/main/rovbase-no-1y.csv
```

Link it as an external dataset and the map refreshes as this repo updates. For the larger files,
turn on *Refine → Symbol shape and size → Group nearby symbols* (hex binning) — an ungrouped
90k-point map is slow and unreadable.

## Columns

| Column | Notes |
|---|---|
| `id` | Rovbase catalogue number. `K…` = livestock damage, `M…` = dead carnivore |
| `lat`, `lon` | WGS84 (EPSG:4326), from the Darwin Core export |
| `title` | e.g. `Rein skadet av gaupe`, `Død gaupe` |
| `comment` | Full assessment, e.g. `Ett dyr er  drept. Skaden er undersøkt av SNO. …` |
| `art` | Carnivore species (Gaupe, Jerv, Bjørn, Ulv, Kongeørn) |
| `bytte` | Animal damaged (Sau, Rein, Hund, Geit, Storfe). **Empty for `DodeRovdyr`** — a dead carnivore has no prey |
| `dato`, `aar` | Event date and year — `aar` is handy for filtering/animating |
| `kommune` | Municipality, suffixed `(N)` Norway, `(S)` Sweden, `(F)` Finland |
| `funnsted` | Locality |
| `datatype` | `Rovviltskade` (damage) or `DodeRovdyr` (dead carnivore) |
| `vurdering` | Assessment: Dokumentert, Antatt sikker, Usikker, Feilmelding |

Two quirks worth knowing:

- The double space in `Ett dyr er  drept` is upstream's own — reproduced deliberately so the text
  matches rovbase.no character-for-character.
- ~74 records carry an empty `title`/`comment`. They exist in the Darwin Core export but were
  withdrawn from the live Rovbase API. They're kept so the record set stays complete; they are
  absent from the `-no*` files, being Swedish.

## Source and licence

Data: [Rovbase](https://www.rovbase.no) / Miljødirektoratet, via
[GBIF Norway](https://ipt.gbif.no/resource?r=rovbase) (dataset
`3b45d150-bb41-488d-a438-9e7ffb3c6101`). Check the upstream dataset for licence and citation
terms before republishing. This repo is a derived convenience copy — please credit Rovbase /
Miljødirektoratet.
