# Public Datasets

A small collection of datasets I've gathered (hoarded) for personal projects. Each pulled from an official or public source, cleaned and flattened into tidy CSVs, and published here as **one file per year**.

Everything is extracted and published automatically by a self-hosted [Dagster](https://dagster.io) pipeline running on a Raspberry Pi, and **refreshed monthly**. Updates are idempotent, a year whose source data hasn't changed produces no new commit, so this repo's commit history doubles as a changelog of what actually changed.

## Datasets

| Dataset | What's in it | Coverage |
|---------|--------------|----------|
| [**aviation-safety/**](aviation-safety/) | Worldwide aviation accidents & incidents from the Aviation Safety Network, one row per occurrence (aircraft, operator, casualties, location, phase of flight). | 2000–present |
| [**pdx-crime/**](pdx-crime/) | Portland Police Bureau reported crime offenses, one row per incident-offense (location, category, offense type). | 2015–present |
| [**pdx-vehicles/**](pdx-vehicles/) | Portland stolen vehicles broken down by make + model, per neighborhood, per month. | 2015–present |
| [**pdx-camps/**](pdx-camps/) | Portland campsite-ordinance enforcement, one flat row per neighborhood per month (cleanups, contacts, outcomes, warrants). | 2025–present |

Each folder includes a `README.md` with a full column reference.

## Format

```
<dataset>/
  <name>_<YYYY>.csv      # one UTF-8 CSV per year, with a header row
```

Each year's file is rebuilt wholesale, so it always reflects the latest values from
the source — prior years can change when a source revises its records.
