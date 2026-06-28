# Portland Concert Events

Upcoming (and recently past) **concerts and live events** at ~15 Portland-area
venues -- Mississippi Studios, Revolution Hall, Aladdin Theater, Crystal Ballroom,
Wonder Ballroom, Roseland Theater, Hawthorne Theatre, Holocene, The 1905, Edgefield,
and the rooms they cross-promote. One row per show, with supporting acts normalized
into a companion file.

The data is scraped daily from each venue's public listings/API and published here
by [dagster-pi](https://github.com/MDeanLindsay/dagster-pi). Because the scrapers
only return *upcoming* shows, the dataset accumulates: a one-time historical seed
plus everything scraped since, so past shows persist instead of disappearing once
they happen.

## Files

Two files per year, keyed on `event_date`:

- `concert_shows_<YYYY>.csv` -- one row per show.
- `concert_show_supporting_acts_<YYYY>.csv` -- one row per supporting act, linked to
  a show by `(venue_slug, artist_name, event_date)`.

## `concert_shows` columns

| Column | Type | Description |
|--------|------|-------------|
| `venue_slug` | VARCHAR | Venue identifier (e.g. `mississippi-studios`, `holocene`). **Primary key** with `artist_name` + `event_date`. |
| `artist_name` | VARCHAR | Headlining artist / event title, cleaned of status tags and encoding glitches. **Primary key** part. |
| `event_date` | DATE | Local calendar date of the show. **Primary key** part. |
| `doors_time` | TIME | Doors time (local), when published; otherwise null. |
| `show_time` | TIME | Set/show start time (local), when published; otherwise null. |
| `ticket_url` | VARCHAR | Link to the event/ticket page. Often null. |
| `poster_url` | VARCHAR | Event poster image URL. Often null. |
| `description` | VARCHAR | Free-text blurb where the source provides one (mostly The 1905); usually null. |
| `is_low_value` | BOOLEAN | `true` for theme/format events (dance & DJ nights, tribute acts, drag, karaoke, trivia, etc.) rather than a billed touring artist. Filter with `WHERE NOT is_low_value` for "real" shows. |

## `concert_show_supporting_acts` columns

| Column | Type | Description |
|--------|------|-------------|
| `venue_slug` | VARCHAR | Links to the parent show. **Primary key** part. |
| `artist_name` | VARCHAR | Links to the parent show. **Primary key** part. |
| `event_date` | DATE | Links to the parent show. **Primary key** part. |
| `billing_order` | INTEGER | 1-based position in the support billing (1 = first listed). **Primary key** part. |
| `support_act` | VARCHAR | A single supporting act, split out from the source's comma-joined list. |

## Notes & caveats

- **Future-dated & accumulating.** Files cover upcoming shows; a year's file grows as
  more of that year goes on sale, and is rebuilt wholesale each publish (times, links,
  and support lineups for a show are corrected in place, never duplicated).
- **Cross-posted shows.** Some venues (notably Aladdin) advertise shows at other rooms;
  those carry the *other* room's `venue_slug`, so a show can appear under the venue that
  actually hosts it.
- **`is_low_value` is a heuristic.** It's a best-effort rule over the event title (keeps
  multi-night residencies, real bands with words like "party" in the name); spot-check
  before relying on it. Tribute acts are flagged; comedy is not.
- **Supporting-act splitting.** Acts come from free-text fields split on commas, so an act
  whose name itself contains a comma can be over-split. Internal "&" names are preserved.

## Attribution

Compiled from the public event listings of the individual venues. Credit the venues as the
source, and review each venue's terms before redistributing.
