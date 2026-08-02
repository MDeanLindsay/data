# Oregon Nonprofits

Every **nonprofit corporation registered with the Oregon Secretary of State** — active
domestic and foreign nonprofits, back to an 1850 church registration — flattened to one
row per organization.

The state publishes this as one row per *associated name* (mailing address, principal
place of business, registered agent, president, secretary), repeating each org's details
five-ish times over ~182,000 rows. Here it's collapsed to **one row per registry number**,
with a single address block chosen per the rules below.

The data is refreshed weekly from the state's open-data export and published here by
[dagster-pi](https://github.com/MDeanLindsay/dagster-pi).

## File

- `oregon-nonprofits.csv`: one row per registered nonprofit.

## Columns

| Column | Description |
|--------|-------------|
| `registry_number` | State registry number — the unique id for the organization. |
| `business_name` | Registered name of the nonprofit. |
| `entity_type` | `DOMESTIC NONPROFIT CORPORATION` or `FOREIGN NONPROFIT CORPORATION`. |
| `registry_date` | Date the organization was registered with the state. |
| `nonprofit_type` | Public benefit, mutual benefit, or religious — with or without members. |
| `address_type` | Which associated-name record the address came from (see below). |
| `address` | Street address. |
| `address_continued` | Suite / unit / second address line, when present. |
| `city` | City. |
| `state` | Two-letter state or province code. |
| `zip_code` | Postal code — not numeric; foreign registrations carry Canadian codes. |

## How the address is chosen

An organization can have several addresses on file. We take the first available of:

1. `MAILING ADDRESS` — the org's correspondence address; present for the vast majority.
2. `PRINCIPAL PLACE OF BUSINESS` — where it actually operates.
3. `REGISTERED AGENT` — always on file, so it's the backstop. Often an agent service or
   law firm rather than the organization itself.
4. `PRESIDENT`, then `SECRETARY` — officer addresses, last resort.

`address_type` records which one won, so rows resting on a fallback are easy to spot
or filter out.

## Caveats

- A snapshot, not a history: the file reflects the registry as of the last refresh.
  Addresses change in place and dissolved registrations drop out.
- Officer and registered-agent *names* are in the source but not here — this dataset is
  about organizations, not the people attached to them.
- `registry_date` keeps the date only; the source's filing timestamp is dropped.

## Attribution

Source: **Oregon Secretary of State**, via [data.oregon.gov](https://data.oregon.gov/)
(dataset `8kyv-b2kw`). Public record data, republished as-is apart from the collapse
described above.
