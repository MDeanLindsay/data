# Portland Stolen Vehicle Data

Accessed directly from Portland Police Bureau's Open Data initiative.

**Source:** [Monthly Stolen Vehicle Statistics](https://public.tableau.com/app/profile/portlandpolicebureau/viz/MonthlyStolenVehicleStatistics/Dashboard)

---

## About This Dataset

One row per **make + model** stolen, per **neighborhood**, per **month**. Every row's `stolen_count` for a given neighborhood-month sums back to the count the dashboard publishes for that neighborhood-month — no double counting, no gaps.

The source dashboard does **not** expose a clean make-and-model fact table. It publishes three separate, partial aggregates per neighborhood-month: a **top-5 makes** list, a **top-10 models** list, and a **grand total**. The model labels do not reliably carry their make, and neither list is exhaustive. This output reconstructs the lowest usable grain (make + model) by attributing models to makes and then filling the gaps the partial lists leave behind with explicit `Unknown` rows (see below).

---

## Column Info

| Field | Description |
|-------|-------------|
| **neighborhood** | Portland neighborhood where the vehicle was reported stolen. |
| **month_start** | First day of the month the statistics cover (YYYY-MM-01). `stolen_count` is the monthly total for that neighborhood + make + model. |
| **make** | Vehicle manufacturer. Derived by matching the model label against the dashboard's own make list (see *How makes are derived*). May be `Unknown` when the make cannot be attributed. |
| **model** | Vehicle model label as published by the dashboard. May be `Unknown` when only a make-level count is known (see *How Unknown works*). |
| **stolen_count** | Number of vehicles of this make + model reported stolen in this neighborhood that month. |

---

## How makes are derived

Because model rows arrive without a clean make field, each model is attributed to a make by matching its label against the dashboard's published list of makes, in priority order:

1. **Model starts with a make** — e.g. `Honda Civic, CRX, Del Sol` → `Honda`. (Highest priority.)
2. **A make starts with the model** — handles truncated labels where the model string is itself a shortened make name (minimum 3 characters, to avoid spurious short matches).
3. **`Unknown X Model` pattern** — for labels shaped like `Unknown Toyota Model`, the inner name (`Toyota`) is extracted and re-matched against the make list.

When more than one make matches, the longest (most specific) make wins. If a model still has no match after all three passes, its make falls back to the unvalidated inner label from the `Unknown X Model` pattern, and failing that, to `Unknown`.

## How `Unknown` works

The partial top-N lists never fully explain a neighborhood-month's total, so `Unknown` is used deliberately to make every neighborhood-month reconcile exactly to the published grand total. It appears in three distinct situations:

- **Known make, unknown model** (`make = Honda`, `model = Unknown`): the dashboard's make-level count for that make exceeds what its listed top-10 models account for. The residual — vehicles of a known make whose specific model falls outside the top-10 — is emitted as a single `Unknown`-model row for that make.
- **Unknown make and model** (`make = Unknown`, `model = Unknown`): the neighborhood-month total exceeds everything the top-5 makes and attributed models explain. This tail — vehicles whose make never appeared in the top-5 — is emitted as a fully `Unknown` row so the totals still add up.
- **Unvalidated make label**: when a model arrived as `Unknown X Model` but `X` did not match any known make, `X` is kept as the make label as-is rather than discarded.

Because of this design, **summing `stolen_count` across all rows for a neighborhood-month reproduces the dashboard's published total for that neighborhood-month.**
