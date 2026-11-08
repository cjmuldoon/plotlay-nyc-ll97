# The data

All of it is made from NYC Open Data (see [`../NOTICE`](../NOTICE)); our tables and shapes are CC BY 4.0.

## tables/

- `properties.csv`: one row per kept LL84 record for calendar 2024 (27,270 rows). Key: `property_id` (Portfolio Manager). `bbl` is the first lot the record lists, `pluto_bbl` the one that matched PLUTO; `cd` the community district (three digits: borough then district); `lat`, `lng` the lot's PLUTO position. `gfa_uses` is the floor area by use in square feet (the limit's base); `emissions_*`, `limit_*`, `over_*` are tonnes of CO2e (tCO2e) for 2024 to 2029 (`_2024`) and 2030 to 2034 (`_2030`); `penalty_*` US dollars a year at $268 a tonne; `ratio_*` emissions over limit. `ll84_ghg` is Portfolio Manager's own location-based figure, for comparison. `pathway` is Article 320 or the likely other route; `check` is blank when the record passes the data checks; `in_totals` is both. Owner names are not included.
- `properties_by_period.csv`: the properties in the totals with a position, one row per period (`period_label`), as linked in Plotlay by position.
- `midtown_by_period.csv`: every benchmarked property in Manhattan Community District 5 with a footprint, one row per period, with its `status`.
- `excluded.csv`: every LL84 record left out, and why.
- `emissions_factors.csv`: the rule's factor per Portfolio Manager property type for both periods (tCO2e per square foot), with the LL84 names that take each (`ll84_names`).
- `coefficients.csv`: the coefficient used for each LL84 energy column, both periods.
- `community_districts.csv`: the 59 districts, their names and boroughs.

## geometry/

- `community_districts.*`: the 59 districts picked from the City Planning file in Plotlay, keyed by `boro_cd`.
- `midtown_buildings.*`: 1,530 building outlines from the city's Building Footprints, keyed by the LL84 `property_id` (a property with several buildings has several parts), with `height_m`.

Units: square feet (sf) and kBtu as LL84 publishes them; tonnes of CO2e; US dollars. 1 sf = 0.0929 m².
