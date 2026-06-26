# Aviation Safety Network — `asn_accidents`

A single flat DuckDB table with **one row per aviation occurrence** (accident,
serious incident, hijacking, or other notable event) recorded by the Aviation
Safety Network. Coverage runs **2000 to the present** — roughly 200–320 records
per year, ~6,800+ rows in total.

Data originates from the **Aviation Safety Network**, a service of the **Flight
Safety Foundation** (<https://asn.flightsafety.org>). It is collected from ASN's
public record pages. Credit ASN/FSF when reusing this dataset, and review ASN's
terms of use for any redistribution.

---

## Data source

- **Aviation Safety Network (ASN)** — <https://asn.flightsafety.org>, a public
  database operated by the **Flight Safety Foundation**. It catalogs civil and
  military aviation occurrences worldwide, with a record page per event.
- **Grain:** one row = one ASN occurrence record.
- **Identity:** every record has a stable, unique source URL in one of two shapes
  that have coexisted for years — a newer Wikibase id (`/wikibase/349536`) and an
  older date-based id (`/database/record.php?id=20000103-0`). We split that URL
  into the atomic `record_source` + `record_id` pair, which together form the
  table's primary key. (~75% of historical rows are `record_php`, ~25% `wikibase`.)
- **Freshness:** ASN keeps adding and revising records for a while after an event,
  so recent years are re-pulled periodically; a re-pull replaces that year wholesale.

---

## Columns

27 columns: 24 of source data, plus 3 of pipeline bookkeeping at the end. Types are
DuckDB types. "Often null" notes reflect how sparsely ASN populates the field
across the full archive.

| Column | Type | Description |
|--------|------|-------------|
| `record_id` | VARCHAR | Record id within its source namespace (e.g. `349536`, `20000103-0`). **Primary key** with `record_source`. |
| `record_source` | VARCHAR | Which ASN URL scheme the record came from: `wikibase` or `record_php`. **Primary key** with `record_id`. |
| `accident_date` | DATE | Date of the occurrence, parsed from ASN's date string. Null (~1%) when ASN only gives a partial date (e.g. `"xx Oct 2001"`). |
| `accident_date_raw` | VARCHAR | The original ASN date string, verbatim — preserves partials that don't parse. |
| `accident_time` | VARCHAR | Local time of the occurrence (e.g. `17:47`), kept as text; may carry an `LT` suffix. Often null (~19%). |
| `aircraft_type` | VARCHAR | Aircraft make/model/variant (e.g. `Airbus A350-941`). |
| `owner_operator` | VARCHAR | Owner or operator of the aircraft (e.g. `Japan Airlines`). |
| `registration` | VARCHAR | Aircraft registration / tail number. Occasionally null (~3%). |
| `msn` | VARCHAR | Manufacturer's serial number. Occasionally null (~4%). |
| `year_of_manufacture` | INTEGER | Year the airframe was built. Often null (~10%). |
| `engine_model` | VARCHAR | Engine make/model. Often null (~22%). |
| `fatalities` | INTEGER | People **aboard** who died (split from ASN's combined fatalities cell). |
| `occupants` | INTEGER | Total people **aboard** (split from the same cell). Often null (~12%) when ASN didn't record it. |
| `other_fatalities` | INTEGER | People **not aboard** who died (e.g. on the ground). |
| `aircraft_damage` | VARCHAR | Damage outcome (e.g. `Destroyed, written off`, `Substantial`). |
| `category` | VARCHAR | ASN occurrence category (e.g. `Accident`, `Incident`, `Serious incident`, `Unlawful Interference`). |
| `location` | VARCHAR | Free-text location of the occurrence, usually with country. |
| `phase` | VARCHAR | Flight phase (e.g. `Landing`, `Takeoff`, `En route`, `Taxi`). |
| `nature` | VARCHAR | Flight nature/purpose (e.g. `Passenger - Scheduled`, `Military`, `Cargo`). |
| `departure_airport` | VARCHAR | Departure airport (name + codes). Often null (~11%). |
| `destination_airport` | VARCHAR | Destination airport (name + codes). Often null (~11%). |
| `investigating_agency` | VARCHAR | Agency investigating the occurrence (e.g. `JTSB`, `NTSB`). Frequently null (~40%). |
| `confidence_rating` | VARCHAR | ASN's note on how well-sourced the record is (e.g. *"Information is only available from news, social media or unofficial sources"*). Often null (~12%). |
| `url` | VARCHAR | Full source URL of the ASN record (provenance; the key is derived from it). |
| `data_year` | INTEGER | The year this row belongs to — the partition / scrape key used for delete-and-replace. Equals the `accident_date` year for parseable dates. |
| `source` | VARCHAR | *Pipeline provenance:* `archive_csv` (historical seed) or `scrape` (live pull). |
| `ingested_at` | TIMESTAMP | *Pipeline bookkeeping:* UTC time the row was loaded. |

---

## Notes & caveats

- **Casualty semantics.** `fatalities` and `occupants` count people **aboard**;
  `other_fatalities` counts people **not aboard**. Total deaths for an event are
  `fatalities + other_fatalities`. By construction `fatalities <= occupants` when
  both are known.
- **Partial dates.** ~1% of older records carry an imprecise date ASN itself
  couldn't pin down; for these `accident_date` is null but `accident_date_raw`
  keeps whatever ASN published.
- **Sparsity.** Several descriptive fields are blank for a meaningful share of
  records (see the "often null" notes above) — ASN populates them as information
  becomes available, so coverage is better for recent and well-investigated events.
- **Revisions.** ASN updates records over time; the recent years are re-scraped
  and replaced wholesale, so counts and details for a given year can shift between
  pulls. The `(record_source, record_id)` key stays stable.
- **Confidence rating.** Treat low-confidence records (news/social-media-sourced)
  accordingly — they're included for completeness, not as confirmed findings.

---

## Attribution

Data originates from the **Aviation Safety Network**, a service of the **Flight
Safety Foundation** (<https://asn.flightsafety.org>). It is collected from ASN's
public record pages. Credit ASN/FSF when reusing this dataset, and review ASN's
terms of use for any redistribution.
