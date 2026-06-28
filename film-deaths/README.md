# Film Deaths

One row per on-screen film death catalogued by the [Cinemorgue wiki](https://cinemorgue.fandom.com/), a crowd-sourced record of how characters die in media. Data is rebuilt monthly from Cinemorgue's public database dump and published here by [dagster-pi](https://github.com/MDeanLindsay/dagster-pi). TV, video-game, and other non-film media are excluded. Source: Cinemorgue Wiki on Fandom (<https://cinemorgue.fandom.com>), licensed CC BY-SA.

## Columns

| Column | Description |
|--------|-------------|
| `actor` | The actor/actress who died (the Cinemorgue page title). |
| `movie` | Film title. |
| `film_year` | Release year; disambiguates remakes of the same title. |
| `cause_of_death` | Classified cause category, or `Other` if nothing matched. One of: Firearms, Cutting Instrument, Blunt Objects, Personal Weapons, Strangulation, Drowning, Impact, Vehicular, Fire, Explosives, Poison, Narcotics, Animals, Supernatural, Weather, Ailment, or `Other`. |
| `death_description` | The cleaned wiki entry the cause was derived from; handy for spot-checking. |

## Caveats

- `cause_of_death` is inferred from the entry's free-text death note (first matching keyword wins). Treat it as a best-effort label, not gospel.
- Crowd-sourced free text: expect the occasional imperfect title or misclassified cause.
- Entries with no parseable release year are dropped.
- If the file ever grows too large to publish, it is trimmed to actors with the most deaths (one-off deaths drop first), so the "who died the most" leaderboard stays intact.
