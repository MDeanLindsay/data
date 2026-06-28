# Portland Stolen Vehicle Data

One row per make + model stolen, per neighborhood, per month, published by the Portland Police Bureau. Data accessed from PPB's Open Data initiative: [Monthly Stolen Vehicle Statistics](https://public.tableau.com/app/profile/portlandpolicebureau/viz/MonthlyStolenVehicleStatistics/Dashboard).

## Columns

| Column | Description |
|--------|-------------|
| `neighborhood` | Portland neighborhood where the vehicle was reported stolen. |
| `month_start` | First day of the month the statistics cover (YYYY-MM-01). `stolen_count` is the monthly total for that neighborhood + make + model. |
| `make` | Vehicle manufacturer. Derived by matching the model label against the dashboard's make list. May be `Unknown` when the make cannot be attributed. |
| `model` | Vehicle model label as published by the dashboard. May be `Unknown` when only a make-level count is known. |
| `stolen_count` | Number of vehicles of this make + model reported stolen in this neighborhood that month. |

## Caveats

- The source dashboard publishes three partial aggregates per neighborhood-month (top-5 makes, top-10 models, grand total) rather than a clean make-and-model table. Make attribution and gap-filling are performed in the pipeline.
- Makes are derived by matching each model label against the dashboard's published makes list in priority order: (1) model starts with a make name, (2) a make name starts with the model label (minimum 3 characters), (3) label matches the "Unknown X Model" pattern. When multiple makes match, the longest wins.
- `Unknown` rows are emitted deliberately so every neighborhood-month reconciles to the published grand total. `make = Honda, model = Unknown` means residual Hondas not in the top-10 models; `make = Unknown, model = Unknown` means vehicles whose make never appeared in the top-5.
- Summing `stolen_count` across all rows for a neighborhood-month reproduces the dashboard's published total for that neighborhood-month.
