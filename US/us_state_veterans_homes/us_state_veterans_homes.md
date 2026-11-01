# US State Veterans Homes

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 173](https://img.shields.io/badge/records-173-lightgrey) ![updated 2026-10-19](https://img.shields.io/badge/updated-2026--10--19-orange)

All 173 State Veterans Homes on the U.S. Department of Veterans Affairs (VA) State Home Per Diem Program list, in the 50 states and Puerto Rico. Each row has the home name, VA region (VISN) and station, street address, city, county, state, whether VA counts the home as urban or rural, the FY26 and FY27 per diem rates VA lists for the home in US dollars per day, and a map point. Filter by the required `state`, then by city, county, name, VISN, urban or rural, a rate range or a coordinate range.

Rows by area: Urban 116 and Rural 57. Most homes: Texas (10), Florida (8), Minnesota, Missouri and Oklahoma (7 each). VA last updated the source page on 2026-10-19.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/us_state_veterans_homes` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`state`). Optional: 13 fields. Disallowed: 3 fields.

`state` must be in every call. Use `state=*` to list all 173 homes. Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `state` | Required | Yes | No | 2 letter postal code, 51 codes (the 50 states and `PR` for Puerto Rico), such as `TX` (10), `FL` (8) or `PR` (1). Wildcards work, so `state=*` lists all 173 homes and `state=M*` matches MA, MD, ME, MI, MN, MO, MS and MT (33). |
| `facility_name` | Optional | Yes | No | Home name as VA lists it, such as `*Missouri*` (7) or `*Veterans Home*` (114). Names repeat within a state (all 7 Missouri rows are `Missouri Veterans Home`), so use `__id` as the row key. |
| `visn` | Optional | No | No | VA region (Veterans Integrated Service Network) number, from 1 to 23, as text, such as `19` (21 homes) or `23` (16). Exact match only. |
| `station` | Optional | No | No | VA station number that oversees the home, as text, such as `589` (6). Exact match only. Null for 1 row (Arizona State Veterans Home - Flagstaff), where VA gives none. |
| `address` | Optional | Yes | No | Street address in upper case as VA lists it, such as `*VETERANS*`. A few rows give only a road, such as `HIGHWAY 183 S` or `CARR.592`. |
| `city` | Optional | Yes | No | City in upper case as VA lists it, such as `SAINT*` (3) or `*BEACH*` (2). |
| `county` | Optional | Yes | No | County in upper case as VA lists it, such as `LOS ANGELES` or `ST.*` (4). Some Virginia rows say `CITY`, such as `RICHMOND CITY`. |
| `state_name` | Optional | Yes | No | Full state name, added from the postal code, such as `Texas` or `*Carolina*` (10). |
| `urban_rural` | Optional | No | No | `Urban` (116) or `Rural` (57), as VA classes the home. Exact match only. |
| `fy26_rate` | Optional | No | Yes | FY26 per diem rate VA lists for the home, in US dollars per day, from 329.92 to 825.59. Example: `fy26_rate=500..600`. |
| `fy27_rate` | Optional | No | Yes | FY27 per diem rate VA lists for the home, in US dollars per day, from 340.48 to 852.01. Example: `fy27_rate=600..900` (47). |
| `latitude` | Optional | No | Yes | Latitude in decimal degrees (WGS84), from 18.053345 to 61.597734. Example: `latitude=40..45`. Check `geocode_quality` before using a point as an exact pin. |
| `longitude` | Optional | No | Yes | Longitude in decimal degrees (WGS84), from -158.06783 to -66.504734. Example: `longitude=-90..-80`. |
| `geocode_quality` | Optional | No | No | How the map point was found: `census_exact` (104), `census_non_exact` (28), `nominatim_address` (14), `nominatim_street` (15), `nominatim_poi` (3), `census_tie` (1), `city_centroid` (7) or `county_centroid` (1). See the notes below. Exact match only. |
| `geocode_match` | Disallowed | No | No | The address, place or map feature the geocoder matched, for checking the point. |
| `source` | Disallowed | No | No | Data source and credit for the row: U.S. Department of Veterans Affairs, with the page link. |
| `updated` | Disallowed | No | No | Date VA last updated the source page, `YYYY-MM-DD` (`2026-10-19` for every row). |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.
* A call without `state` returns HTTP 400 with `Your request is missing the following required parameters: state. You did not supply any.`
* A filter on `geocode_match`, `source` or `updated` returns HTTP 400 with `Request attempts to filter on disallowed parameters: source` (or the field you used).

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `facility_name` | string | Home name as VA lists it. |
| `visn` | string | VA region (VISN) number. |
| `station` | string | VA station number. Null for 1 row. |
| `address` | string | Street address, upper case. |
| `city` | string | City, upper case. |
| `county` | string | County, upper case. |
| `state` | string | 2 letter postal code. |
| `state_name` | string | Full state name. |
| `urban_rural` | string | `Urban` or `Rural`. |
| `fy26_rate` | number | FY26 per diem rate in US dollars per day. |
| `fy27_rate` | number | FY27 per diem rate in US dollars per day. |
| `latitude` | number | Latitude in decimal degrees. |
| `longitude` | number | Longitude in decimal degrees. |
| `geocode_quality` | string | How the map point was found. |
| `geocode_match` | string | What the geocoder matched. |
| `source` | string | Data source and credit. |
| `updated` | string | Date VA last updated the source page. |

## Notes

* **173 homes, not 177.** VA's State Veterans Home Program page counts 177 homes as of June 1, 2026, but the per diem table this API uses lists 173. The other homes in VA's count are not in that table, so they are not in this API.
* **Rates.** `fy26_rate` and `fy27_rate` are the figures VA shows in the columns "FY26 MB 3.2% Calculation Rate" and "FY27 MB 3.2% Calculation Rate". In this table every FY27 rate is the FY26 rate times 1.032. They are VA's per diem figures for each home, not a quote of what a resident pays, so ask the home about its own charges.
* **Map points.** The source has no coordinates, so every row was geocoded. `census_exact` and `census_non_exact` are US Census Geocoder address matches. `nominatim_address` is an OpenStreetMap address match, `nominatim_street` is a point on the right street but not at the door, and `nominatim_poi` is the home as mapped in OpenStreetMap. `census_tie` (Louisiana War Veterans Home, Jackson) matched two Census records, and the `4739 HWY 10` match is used. `city_centroid` and `county_centroid` are the center of the city or county from the 2024 Census Gazetteer, used for 8 homes no geocoder could place. Every point was checked to fall in its state. Do not use street, tie or centroid rows as exact pins.
* **VA text kept as is.** Names, addresses, cities and counties are kept the way VA wrote them, typos included. Examples: `400 VETERNS DR` (Columbia Falls, MT), the city `ALEXANDRIA CITY` (Alexander City, AL), the city `DELAWARE VALLEY` (the home is in Philadelphia, PA), `North Carolina Veterans Home - Kingston` in the city of `KINSTON`, and cities with a note in brackets such as `BELLEVUE (Omaha)`, `MENLO PARK (Edison)`, `SPRINGFIELD (Panama City)` and `HOMELAKE (Monte Vista)`.
* **County mismatches.** For a few homes the county VA lists is not the county the map point falls in: Fitzsimons in Aurora, CO (listed Arapahoe, point in Adams), Jennings, LA (listed Acadia, point in Jefferson Davis), Charlotte Hall, MD (listed Charles, point in St. Mary's), E. Roy Stone, Jr. War Veterans Pavilion in Columbia, SC (listed Calhoun, point in Richland) and Andre Copas State Veterans Home in Port St. Lucie, FL (listed Martin, street point in St. Lucie). Milford, DE sits on the Kent and Sussex line. The `county` field keeps VA's value.
* **Hollidaysburg, PA.** VA lists 138 Veterans Blvd, while the OpenStreetMap point used here is the home at 500 Municipal Drive.

## Examples

All homes in one state. This call returns 10 rows:

```bash
curl -s "https://api.jael.ee/JLEE/us_state_veterans_homes?state=TX"
```

```json
[
  {
    "__id": 146,
    "facility_name": "Ussery-Roan Texas State Veterans Home",
    "visn": "17",
    "station": "504",
    "address": "1020 TASCOSA ROAD",
    "city": "AMARILLO",
    "county": "POTTER",
    "state": "TX",
    "state_name": "Texas",
    "urban_rural": "Urban",
    "fy26_rate": 522.72,
    "fy27_rate": 539.45,
    "latitude": 35.219368,
    "longitude": -101.909445,
    "geocode_quality": "census_exact",
    "geocode_match": "1020 TASCOSA RD, AMARILLO, TX, 79124",
    "source": "U.S. Department of Veterans Affairs, State Home Per Diem Program (https://www.va.gov/GERIATRICS/State_Home_Per_Diem_Program.asp)",
    "updated": "2026-10-19"
  },
  ...
]
```

A wildcard on the name across all states. This call returns 7 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/us_state_veterans_homes" --data-urlencode "state=*" --data-urlencode "facility_name=*Missouri*" | jq -r '.[] | [.facility_name, .city, .urban_rural, .fy27_rate] | @tsv'
```

```text
Missouri Veterans Home	MOUNT VERNON	Rural	520.66
Missouri Veterans Home	MEXICO	Rural	520.66
Missouri Veterans Home	CAMERON	Urban	580.81
Missouri Veterans Home	WARRENSBURG	Rural	520.66
Missouri Veterans Home	SAINT JAMES	Rural	520.66
Missouri Veterans Home	CAPE GIRARDEAU	Urban	531.65
Missouri Veterans Home	SAINT LOUIS	Urban	595.05
```

A range on the FY27 rate. Homes with an FY27 rate from $750 to $900 a day. This call returns 8 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/us_state_veterans_homes" --data-urlencode "state=*" --data-urlencode "fy27_rate=750..900" | jq -r '.[] | [.facility_name, .city, .state, .fy26_rate, .fy27_rate] | @tsv'
```

```text
Veterans Home of California, Redding	REDDING	CA	765.91	790.42
Veterans Home of California	YOUNTVILLE	CA	825.59	852.01
Veterans Home of California	LOS ANGELES	CA	732.04	755.47
Daniel K Akaka State Veterans Home	KAPOLEI	HI	733.97	757.46
Paramus Veterans Home	PARAMUS	NJ	735.89	759.44
Menlo Park Veterans Memorial Home	MENLO PARK (Edison)	NJ	735.89	759.44
New York State Veterans' Home at Montrose	MONTROSE	NY	735.67	759.21
St. Albans Veterans Home	ST. ALBANS	NY	735.93	759.48
```

A wildcard on the state code. This call returns 33 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/us_state_veterans_homes" --data-urlencode "state=M*" | jq length
```

## Data source

The home list and rates come from the U.S. Department of Veterans Affairs [State Home Per Diem Program](https://www.va.gov/GERIATRICS/State_Home_Per_Diem_Program.asp) page, last updated by VA on 2026-10-19 and fetched on 2026-11-02. It is a work of the US federal government and is in the public domain. Under [17 U.S.C. 105](https://www.law.cornell.edu/uscode/text/17/105), "Copyright protection under this title is not available for any work of the United States Government". Changes: columns renamed, rates turned into numbers, full state names added from the Census state codes, and map points added. The data is not guaranteed to be complete or accurate.

Credit lines:

* Source: U.S. Department of Veterans Affairs, State Home Per Diem Program (https://www.va.gov/GERIATRICS/State_Home_Per_Diem_Program.asp). U.S. government work, public domain.
* Map points: US Census Geocoder (https://geocoding.geo.census.gov), public domain.
* Map points: © OpenStreetMap contributors, through Nominatim, available under the Open Database License (ODbL) (https://www.openstreetmap.org/copyright).
* City and county centers: 2024 Census Gazetteer files (https://www.census.gov/geographies/reference-files/time-series/geo/gazetteer-files.html), public domain.
* Not endorsed by the U.S. Department of Veterans Affairs. No VA logos are used.

[Back to the API list](../../README.md)
