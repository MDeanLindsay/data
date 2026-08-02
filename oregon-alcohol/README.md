# Oregon Alcohol Prices

Monthly price history for **every distilled-spirits product sold in Oregon's
state-controlled liquor stores** — roughly 3,850 items a month, with bottle, case, and
**per-ounce** pricing, straight from the Oregon Liquor & Cannabis Commission's published
price list.

The state publishes only the *current* month: the file is replaced wholesale when new
prices take effect on the 1st, and there is no public archive of prior months. This
dataset exists to keep that history — every month is captured as it's published by
[dagster-pi](https://github.com/MDeanLindsay/dagster-pi) and appended here, so the price
of a bottle can be tracked over time.

## File

- `oregon-alcohol.csv`: one row per item **per month** (`as_of_date` + `item_code`).

## Columns

| Column | Description |
|--------|-------------|
| `as_of_date` | Month the prices took effect — always the 1st. The snapshot key. |
| `item_code` | OLCC item code (unique within a month). |
| `extended_item_code` | Longer code that also encodes the bottle size. |
| `description` | Product name as the OLCC lists it. |
| `category` | OLCC category — `DOMESTIC WHISKEY`, `VODKA`, `TEQUILA`, `CORDIALS`, … |
| `size` | Bottle size label as published (`750 ML`, `1.75 L`, `LITER`). |
| `size_ml` | That size in millilitres — the published labels aren't consistent, so this is the one to compute with. |
| `proof` | Proof (twice the ABV). |
| `age` | Stated age as published (`12 YRS`, `36 MOS`), where the item has one. |
| `age_months` | That age in months, for comparing across units. |
| `country_of_origin` | Country of origin, where recorded. |
| `container_type` | `Bottle`, `Can`, `Multi-Pack Bottle`, `Multi-Pack Can`, `VAP` (value-added package with glassware etc.), `Other`. |
| `container_count` | Containers in the pack. |
| `bottles_per_case` | Bottles per case. |
| `price_per_bottle` | Shelf price of one bottle, in dollars. |
| `price_per_case` | Case price — equals `price_per_bottle` × `bottles_per_case`. |
| `price_per_oz` | Price per fluid ounce, as published by the OLCC. |
| `price_change` | Dollar change from the previous month's bottle price; `0` for most items. |
| `item_status` | `Regular`, `Limited Listing`, `Trial Listing`, `Seasonal`, `One Time Buy`, `Holiday`. |
| `item_status_code` | One-letter form of `item_status`. |
| `oregon_product` | Whether the OLCC flags it as an Oregon product. |
| `new_item` | Whether it's newly listed this month. |
| `special_pricing` | Whether it's on special this month. |

## Caveats

- History starts when this pipeline started capturing it, not when the OLCC started
  publishing — earlier months can't be recovered from the source.
- Prices are the state's list prices for state-controlled stores; they are not
  restaurant, bar, or out-of-state prices.
- The source occasionally leaks spreadsheet errors into `country_of_origin`; those are
  blanked on ingest. A few size labels may not convert to `size_ml`.

## Attribution

Source: **Oregon Liquor & Cannabis Commission**, via
[data.oregon.gov](https://data.oregon.gov/) (dataset `vmf2-f83h`). Public record data,
republished with light typing and cleaning as described above.
