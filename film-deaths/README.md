# Film Deaths

Every on-screen **film death** catalogued by the [Cinemorgue wiki](https://cinemorgue.fandom.com/)
-- a crowd-sourced record of how characters die in media -- flattened to one row per death.
It exists to answer one under-reported question: **which actor has died the most in film?**

The data is rebuilt monthly from Cinemorgue's public database dump and published here by
[dagster-pi](https://github.com/MDeanLindsay/dagster-pi). **TV, video-game, and other media
deaths are excluded** (they're heavily voice roles) -- film only.

## File

- `film-deaths.csv` -- one row per film death.

## Columns

| Column | Description |
|--------|-------------|
| `actor` | The actor/actress who died (the Cinemorgue page title). |
| `movie` | Film title. |
| `film_year` | Release year -- disambiguates remakes of the same title. |
| `cause_of_death` | Classified cause category (see below), or `Other` if nothing matched. |
| `death_description` | The cleaned wiki entry the cause was derived from -- handy for spot-checking. |

`cause_of_death` is one of: Firearms, Cutting Instrument, Blunt Objects, Personal Weapons,
Strangulation, Drowning, Impact, Vehicular, Fire, Explosives, Poison, Narcotics, Animals,
Supernatural, Weather, Ailment -- or `Other`. It is inferred from the entry's free-text death
note (first matching keyword wins), so treat it as a best-effort label, not gospel.

## Caveats

- Crowd-sourced free text: expect the occasional imperfect title or misclassified cause.
- Entries with no parseable release year are dropped.
- If the file ever grows too large to publish, it's trimmed to the actors with the most
  deaths (one-off deaths drop first), so the "who died the most" leaderboard stays intact.

## Attribution

Source: **Cinemorgue Wiki** on Fandom (<https://cinemorgue.fandom.com>), licensed **CC BY-SA**.
Credit Cinemorgue and its contributors, and honor the CC BY-SA terms for any redistribution.
