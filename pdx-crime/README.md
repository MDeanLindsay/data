# Portland Crime Data

One row per incident-offense of reported crime in Portland, published by the Portland Police Bureau. Data accessed from PPB's Open Data initiative: [Monthly Reported Crime Statistics](https://public.tableau.com/app/profile/portlandpolicebureau/viz/MonthlyReportedCrimeStatistics/MonthlyStatistics).

## Columns

| Column | Description |
|--------|-------------|
| `Address` | Address of reported incident at the 100 block level (e.g. 1111 SW 2nd Ave would be 1100 Block SW 2nd Ave). |
| `Case Number` | The case year and number for the reported incident (YY-######). |
| `Crime Against` | Crime against category (Person, Property, or Society). |
| `Neighborhood` | Neighborhood where incident occurred. Blank if the incident occurred outside Portland neighborhood boundaries or could not be assigned to a specific address. |
| `Occur Date` | Date the incident occurred. When the exact date is unknown, the first possible date the crime could have occurred is used. |
| `Occur Time` | Time the incident occurred in 24-hour format (HHMM). When the exact time is unknown, the first possible time is used. |
| `Offense Category` | Category of offense (e.g. Assault Offenses). |
| `Offense Type` | Type of offense (e.g. Aggravated Assault). |
| `Open Data Lat/Lon` | Generalized latitude/longitude of the reported incident, mapped to block midpoint or intersection centroid. |
| `Open Data X/Y` | Generalized XY point using Oregon State Plane North (3601), NAD83 HARN, US International Feet coordinate system. |
| `Offense Count` | Number of offenses per incident. |

## Caveats

- Occur date and time reflect the first possible date/time when the exact moment is unknown (e.g. a burglary discovered after a week-long absence uses the first day of the absence).
- Neighborhood is blank for incidents outside Portland neighborhood boundaries or at locations that could not be assigned to a specific address.
- Points for certain case types are withheld from the XY/Lat-Lon fields to protect victim identity and privacy.
- The Homicide Offenses statistic was updated in 2019 to align with FBI NIBRS definitions, which expanded negligent manslaughter to include traffic fatalities resulting from DUI, distracted driving, or reckless driving. This affects comparability of 2019 homicide statistics to prior years.
