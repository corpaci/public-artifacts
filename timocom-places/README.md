# Timocom place lists

One file per country, `places_<CC>.json`, for the address suggestions on Art-Speed's Timocom offer
page: when a planner types a town or postcode, the page loads that country's file (once) and lists
matching "CC postcode Town" entries; a postcode and town that are not in the list get a short note.

## Source and licence

Postal code data from [GeoNames](https://www.geonames.org/) (`https://download.geonames.org/export/zip/`),
licensed under [Creative Commons Attribution 4.0](https://creativecommons.org/licenses/by/4.0/).
The data is changed as described below. GeoNames gives it "as is", without any warranty.

## Format

UTF-8 JSON, one line:

```
{"c":"HU","n":3571,"d":"2026-10-29","a":"GeoNames (www.geonames.org), CC BY 4.0","p":"1011\tBudapest\n1012\n1013\n..."}
```

- `c` country (ISO 3166 two letters), `n` number of (postcode, town) pairs, `d` date of the GeoNames
  download, `a` source and licence.
- `p` the pairs, one per line, sorted by postcode: `postcode<TAB>town`, or only `postcode` when the
  town is the same as on the line before.

## How the files are made

`node scripts/timocom-places.mjs` in the artspeed-agent repo (`--offline` uses the cached downloads
only, `--check` prints counts and sizes and writes nothing). Changes to the GeoNames data:

- DE: large-customer codes (rows without an accuracy value: companies, offices) are left out.
- FR: CEDEX codes are left out.
- BG: names given as "Cyrillic / Latin" keep the Latin part.
- LU, LV: the "L-" / "LV-" before the number is dropped.
- PT: postcodes are cut to their first 4 digits.
- All: a district number after the town is dropped ("Paris 02" becomes "Paris"); pairs that are then
  the same are kept once.

## Countries

| Country | Pairs | File | Gzipped |
|---|---:|---:|---:|
| AT | 19,001 | 332 KB | 100 KB |
| BE | 2,781 | 47 KB | 18 KB |
| BG | 5,284 | 85 KB | 33 KB |
| CH | 4,344 | 74 KB | 30 KB |
| CZ | 15,503 | 303 KB | 88 KB |
| DE | 15,053 | 265 KB | 92 KB |
| DK | 1,159 | 13 KB | 5.7 KB |
| EE | 5,398 | 81 KB | 31 KB |
| ES | 37,867 | 867 KB | 262 KB |
| FI | 3,576 | 49 KB | 20 KB |
| FR | 37,221 | 761 KB | 250 KB |
| GB | 27,450 | 459 KB | 159 KB |
| HR | 6,774 | 127 KB | 38 KB |
| HU | 3,571 | 58 KB | 24 KB |
| IE | 139 | 2.0 KB | 1.1 KB |
| IT | 18,415 | 353 KB | 112 KB |
| LT | 21,864 | 459 KB | 112 KB |
| LU | 4,518 | 40 KB | 15 KB |
| LV | 6,057 | 97 KB | 33 KB |
| NL | 4,321 | 55 KB | 23 KB |
| NO | 5,132 | 53 KB | 22 KB |
| PL | 72,898 | 1222 KB | 350 KB |
| PT | 34,428 | 654 KB | 169 KB |
| RO | 37,915 | 458 KB | 155 KB |
| RS | 1,155 | 20 KB | 8.4 KB |
| SE | 18,887 | 187 KB | 58 KB |
| SI | 556 | 9.5 KB | 4.2 KB |
| SK | 5,221 | 79 KB | 27 KB |
| CY | 1,127 | 16 KB | 6.8 KB |
| TR | 36,307 | 629 KB | 184 KB |

No file for GR: GeoNames has no postal code data for it.
