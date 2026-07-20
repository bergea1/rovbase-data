# Rovbase — rovdyrregistreringer for kartlegging

Norske og svenske registreringer av store rovdyr (gaupe, jerv, bjørn, ulv, kongeørn),
beriket med skadevurderingsteksten som [rovbase.no](https://www.rovbase.no) viser i
kartverktøytipset, og tilrettelagt for kartverktøy.

**Oppdateres daglig, automatisk.** Bygges på nytt hver gang [Rovbases GBIF-datasett](https://ipt.gbif.no/resource?r=rovbase) publiserer en ny versjon. Hver commit merkes
med GBIF-versjonen den kom fra (`gbif-v1.298`).

## Filer

| Fil | Poster | For |
|---|---|---|
| `rovbase-no-1y.csv` | ~1 700 | Norge, siste 12 måneder — **best for Datawrapper** |
| `rovbase-no-5y.csv` | ~10 600 | Norge, siste 5 år |
| `rovbase-no.csv` | ~74 600 | Norge, hele perioden |
| `rovbase-1y.csv` / `-5y.csv` | ~2 600 / ~16 600 | Alle land, tidsvinduer |
| `rovbase.csv` | ~90 100 | Alt |
| `*.geojson` | — | Samme data som GeoJSON (EPSG:4326) for QGIS / Mapshaper / Felt |
| `versions.jsonl` | — | Én linje per oppstrøms publisering: dato, GBIF-versjon, antall rader, sjekksum |

Tidsvinduene ruller fra dagen filen ble laget, ikke kalenderår.

## Bruk i Datawrapper

Datawrapper **tar symbolkart med CSV med lat/lon-kolonner og godtar ikke GeoJSON** (GeoJSON er
kun for lokaliseringskart, som har et tak på 500 markører). Så pek det mot en CSV:

```
https://raw.githubusercontent.com/bergea1/rovbase-data/main/rovbase-no-1y.csv
```

Koble det til som et eksternt datasett, så oppdateres kartet etter hvert som dette repoet
oppdateres. For de større filene, slå på *Refine → Symbol shape and size → Group nearby symbols*
(heksbinning) — et ugruppert kart med 90k punkter er tregt og uleselig.

## Kolonner

| Kolonne | Merknader |
|---|---|
| `id` | Rovbase-katalognummer. `K…` = husdyrskade, `M…` = dødt rovdyr |
| `lat`, `lon` | WGS84 (EPSG:4326), fra Darwin Core-eksporten |
| `title` | f.eks. `Rein skadet av gaupe`, `Død gaupe` |
| `comment` | Full vurdering, f.eks. `Ett dyr er  drept. Skaden er undersøkt av SNO. …` |
| `art` | Rovdyrart (Gaupe, Jerv, Bjørn, Ulv, Kongeørn) |
| `bytte` | Skadet dyr (Sau, Rein, Hund, Geit, Storfe). **Tomt for `DodeRovdyr`** — et dødt rovdyr har ikke noe bytte |
| `dato`, `aar` | Hendelsesdato og år — `aar` er praktisk for filtrering/animering |
| `kommune` | Kommune, med suffiks `(N)` Norge, `(S)` Sverige, `(F)` Finland |
| `funnsted` | Lokalitet |
| `datatype` | `Rovviltskade` (skade) eller `DodeRovdyr` (dødt rovdyr) |
| `vurdering` | Vurdering: Dokumentert, Antatt sikker, Usikker, Feilmelding |

To særegenheter verdt å kjenne til:

- Det doble mellomrommet i `Ett dyr er  drept` er oppstrøms sitt eget — gjengitt bevisst slik at
  teksten samsvarer med rovbase.no tegn for tegn.
- ~74 poster har en tom `title`/`comment`. De finnes i Darwin Core-eksporten, men ble trukket
  tilbake fra det live Rovbase-API-et. De beholdes slik at datasettet forblir komplett; de er
  fraværende fra `-no*`-filene, siden de er svenske.

## Kilde og lisens

Data: [Rovbase](https://www.rovbase.no) / Miljødirektoratet, via
[GBIF Norge](https://ipt.gbif.no/resource?r=rovbase) (datasett
`3b45d150-bb41-488d-a438-9e7ffb3c6101`). Sjekk oppstrøms-datasettet for lisens- og
siteringsvilkår før du republiserer. Dette repoet er en avledet praktisk kopi — vennligst
krediter Rovbase / Miljødirektoratet.
