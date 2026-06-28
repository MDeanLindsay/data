# Portland Campsite Enforcement

One flat row per neighborhood per month of campsite-ordinance enforcement activity reported by the Portland Police Bureau. Data accessed from PPB's Open Data initiative: [Public Camping Ordinance Enforcement](https://public.tableau.com/app/profile/portlandpolicebureau/viz/PublicCampingOrdinanceEnforcement/PPBCampsiteCleanups).

## Columns

| Column | Description |
|--------|-------------|
| `neighborhood` | Portland neighborhood where the activity occurred (title-case display name). |
| `month_start` | First day of the month the statistics cover (YYYY-MM-01). All measures in the row are monthly totals for that neighborhood. |
| `cleanup_incidents` | Count of distinct campsite-cleanup incidents (cases) in the neighborhood that month. |
| `ordinance_reports` | Count of distinct camping-ordinance reports filed in the neighborhood that month. |
| `cars_contacted` | Cars contacted during cleanups. |
| `cars_cleaned` | Cars cleaned (removed/resolved) during cleanups. |
| `rvs_contacted` | RVs contacted during cleanups. |
| `rvs_cleaned` | RVs cleaned (removed/resolved) during cleanups. |
| `tents_contacted` | Tents contacted during cleanups. |
| `tents_cleaned` | Tents cleaned (removed/resolved) during cleanups. |
| `people_contacted` | Total number of people contacted during enforcement/cleanup activity. |
| `shelter_offered` | Number of times shelter was offered to contacted individuals. |
| `shelter_accepted` | Number of times an offer of shelter was accepted. |
| `warnings_issued` | Number of warnings issued (in lieu of citation or arrest). |
| `arrests_camping_only` | Arrests where the only charge was a camping-ordinance violation. |
| `cites_camping_only` | Citations where the only charge was a camping-ordinance violation. |
| `arrests_camping_other` | Arrests involving a camping-ordinance violation plus additional charges. |
| `cites_camping_other` | Citations involving a camping-ordinance violation plus additional charges. |
| `arrests_other` | Arrests for charges unrelated to the camping ordinance. |
| `cites_other` | Citations for charges unrelated to the camping ordinance. |
| `warrants_failure_to_appear` | Warrants served for failure-to-appear offenses. |
| `warrants_property_crime` | Warrants served for property-crime offenses. |
| `warrants_person_crime` | Warrants served for person-crime (crimes against persons) offenses. |
| `warrants_probation_violations` | Warrants served for probation violations. |
| `warrants_drug_crime` | Warrants served for drug-crime offenses. |
| `warrants_other` | Warrants served that do not fall into the categories above. |

## Caveats

- Neighborhoods with no reported activity in a given month are omitted entirely, not stored as zeros. Within an active neighborhood-month, a category that reported nothing is recorded as `0`.
- The enforcement and warrant measures are parallel statistics, not sub-totals of `cleanup_incidents`. They will not reconcile against the cleanup count.
