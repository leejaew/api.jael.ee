# US Port Health Stations

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 61](https://img.shields.io/badge/records-61-lightgrey) ![updated 2024-05-15](https://img.shields.io/badge/updated-2024--05--15-orange) ![license public domain](https://img.shields.io/badge/license-public%20domain-green)

The 20 CDC port health stations that handle sick travelers and quarantine at US ports of entry, with one row for each state, territory or area a station covers (61 rows). Each row has the station name, the city in its street address, the state or area it covers, any coverage note when CDC splits a state, the street address, the 24 hour phone, a daytime phone where CDC lists one, fax, eFax, the source link and the date on the CDC page. Filter by the required `state`, then by station, station city or coverage area.

Texas has 4 rows (Dallas, El Paso and Houston twice), California has 3 and New York has 2. The other 52 areas have one row each. Coverage notes are filled on 10 rows; the rest are blank because the station covers all ports in that area. The list includes Guam, Puerto Rico, the U.S. Virgin Islands and the US-Affiliated Pacific Islands (USAPI).

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/us_port_health_stations` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`state`). Optional: 3 fields. Disallowed: 7 fields.

`state` must be in every call. Use `state=*` to list all 61 rows. Disallowed means the field is returned in the output but cannot be used as a filter. Without `state` the API returns HTTP 400, and a filter on a disallowed field also returns HTTP 400.

Every field is text, including phone numbers and the date. No field takes a range.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `state` | Required | Yes | No | State, territory or area the station covers, such as `Colorado` (1), `Texas` (4), `Puerto Rico`, `Guam`, `District of Columbia` or `US-Affiliated Pacific Islands (USAPI)`. Wildcards work, so `state=*` lists all 61 rows and `state=New*` gives 5 (New Hampshire, New Jersey, New Mexico and New York twice). Texas, California and New York are split between stations; see `coverage_area`. |
| `station` | Optional | Yes | No | Station name as CDC gives it, one of 20, such as `Atlanta`, `Los Angeles`, `Minneapolis-St. Paul` or `Washington D.C.`. Wildcards work, so `station=San*` gives San Diego, San Francisco and San Juan (6 rows). |
| `station_city` | Optional | Yes | No | City in the station's street address, such as `Jamaica` (New York station at JFK), `East Boston`, `St. Paul`, `Carolina` (San Juan) or `Dulles` (Washington D.C.). Wildcards work. |
| `coverage_area` | Optional | Yes | No | Part of the state a station covers when CDC splits it, such as `North Texas (Health Districts 1, 2, and 3)`, `The entire Texas coastline`, `San Diego and Imperial counties` or `New York City seaports`. Use wildcards, such as `coverage_area=*Texas*` (4). Blank on 51 rows where the station covers all ports in the state. |
| `station_address` | Disallowed | No | No | Street address for FedEx and inside deliveries, as CDC lists it. CDC asks you to confirm the address before shipping with any other carrier. Not filterable. |
| `station_phone_24h` | Disallowed | No | No | Phone number with 24 hour access, as text, such as `310-215-2365`. El Paso and San Diego share `866-638-9753`. Not filterable. |
| `station_phone_daytime` | Disallowed | No | No | Daytime phone, given only for El Paso (`915-834-5950`, Texas and New Mexico) and San Diego (`619-692-5665`, Arizona and California); blank on the other 57 rows. Not filterable. |
| `station_fax` | Disallowed | No | No | Fax number as CDC lists it, with any note kept, such as `Terminal 1: 612-725-3006`. Blank on 23 rows where CDC gives no plain fax. Not filterable. |
| `station_efax` | Disallowed | No | No | eFax number as CDC lists it, such as `678-669-1839`. Blank on 16 rows where CDC gives no eFax. Not filterable. |
| `source` | Disallowed | No | No | Data source: CDC Port Health Station Contact List (`https://www.cdc.gov/port-health/stations/port-health-station-contact-list.html`), a US government work in the public domain. Not filterable. |
| `updated` | Disallowed | No | No | Date on the CDC page, `YYYY-MM-DD` (`2024-05-15`). Not filterable. |

* Parameters are combined with AND. Text matches ignore case, so `state=texas` works the same as `state=Texas`. See [How filters work](../../README.md#how-filters-work) for the wildcard syntax.
* A call without `state` returns HTTP 400 with `Your request is missing the following required parameters: state. You did not supply any.`
* A filter on a disallowed field returns HTTP 400 with `Request attempts to filter on disallowed parameters: station_address`.
* Wildcards work on `state`, `station`, `station_city` and `coverage_area`.

## Response fields

| Field | Type | Description |
|---|---|---|
| `station` | string | Station name as CDC gives it. |
| `station_city` | string | City in the station's street address. |
| `state` | string | State, territory or area the station covers. |
| `coverage_area` | string | Part of the state covered when CDC splits it. Null on 51 rows. |
| `station_address` | string | Street address as CDC lists it. |
| `station_phone_24h` | string | 24 hour phone number. |
| `station_phone_daytime` | string | Daytime phone. Null on 57 rows. |
| `station_fax` | string | Fax number. Null on 23 rows. |
| `station_efax` | string | eFax number. Null on 16 rows. |
| `source` | string | Data source name and link. |
| `updated` | string | Date on the CDC page, `YYYY-MM-DD`. |

Notes:

* No `__id` field. Rows are keyed by the pair of `station` and `state`, or by `station`, `state` and `coverage_area` when Texas or California is split.
* Houston appears twice for Texas: once for the entire Texas coastline and once for East Texas (Health Districts 4, 5, 6 and 7). Both rows share the same address and phones.
* New York is split between the New York station (all ports except New York City seaports) and the Newark station (New York City seaports).

## Examples

Wildcard on the required state. Stations that cover Texas. This call returns 4 rows:

```bash
curl -s "https://api.jael.ee/JLEE/us_port_health_stations?state=Tex*"
```

```json
[
  {
    "station": "Dallas",
    "station_city": "Dallas",
    "state": "Texas",
    "coverage_area": "North Texas (Health Districts 1, 2, and 3)",
    "station_address": "CDC Dallas Port Health Station, c/o Supervisor Rocha, US CBP Terminal D, Arrivals LVL, 2333 South International Parkway, Dallas, TX 75261",
    "station_phone_24h": "972-973-9258",
    "station_phone_daytime": null,
    "station_fax": null,
    "station_efax": "678-623-8021",
    "source": "CDC Port Health Station Contact List, https://www.cdc.gov/port-health/stations/port-health-station-contact-list.html",
    "updated": "2024-05-15"
  },
  {
    "station": "El Paso",
    "station_city": "El Paso",
    "state": "Texas",
    "coverage_area": "West Texas (Health Districts 8, 9, 10, and 11)",
    "station_address": "CDC El Paso Port Health Station, 700 E. San Antonio Ave, Room E-105, El Paso, TX 79901",
    "station_phone_24h": "866-638-9753",
    "station_phone_daytime": "915-834-5950",
    "station_fax": "915-834-5973",
    "station_efax": null,
    "source": "CDC Port Health Station Contact List, https://www.cdc.gov/port-health/stations/port-health-station-contact-list.html",
    "updated": "2024-05-15"
  },
  {
    "station": "Houston",
    "station_city": "Houston",
    "state": "Texas",
    "coverage_area": "The entire Texas coastline",
    "station_address": "CDC Houston Port Health Station, 3870 North Terminal Road (c/o CBP), Bush Intercontinental Airport, Houston, Texas 77032",
    "station_phone_24h": "281-982-9074",
    "station_phone_daytime": null,
    "station_fax": "281-982-9079",
    "station_efax": "404-420-2290",
    "source": "CDC Port Health Station Contact List, https://www.cdc.gov/port-health/stations/port-health-station-contact-list.html",
    "updated": "2024-05-15"
  },
  {
    "station": "Houston",
    "station_city": "Houston",
    "state": "Texas",
    "coverage_area": "East Texas (Health Districts 4, 5, 6 and 7)",
    "station_address": "CDC Houston Port Health Station, 3870 North Terminal Road (c/o CBP), Bush Intercontinental Airport, Houston, Texas 77032",
    "station_phone_24h": "281-982-9074",
    "station_phone_daytime": null,
    "station_fax": "281-982-9079",
    "station_efax": "404-420-2290",
    "source": "CDC Port Health Station Contact List, https://www.cdc.gov/port-health/stations/port-health-station-contact-list.html",
    "updated": "2024-05-15"
  }
]
```

One state covered by a distant station. Colorado is covered by Los Angeles. This call returns 1 row:

```bash
curl -s "https://api.jael.ee/JLEE/us_port_health_stations?state=Colorado"
```

```json
[
  {
    "station": "Los Angeles",
    "station_city": "Los Angeles",
    "state": "Colorado",
    "coverage_area": null,
    "station_address": "CDC Los Angeles Port Health Station, Tom Bradley International Terminal, Public Information Office (PIO desk), Bureau of Customs and Border Protection, Departure Level, Los Angeles, CA 90045 (envelopes only: 380 World Way, Box N-19 380, Los Angeles, CA 90045)",
    "station_phone_24h": "310-215-2365",
    "station_phone_daytime": null,
    "station_fax": "310-215-2285",
    "station_efax": "678-669-1839",
    "source": "CDC Port Health Station Contact List, https://www.cdc.gov/port-health/stations/port-health-station-contact-list.html",
    "updated": "2024-05-15"
  }
]
```

Station name with a wildcard across every state. Stations whose names start with San. This call returns 6 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/us_port_health_stations" --data-urlencode "state=*" --data-urlencode "station=San*" | jq -r '.[] | [.station, .state, .station_phone_24h] | @tsv'
```

```text
San Diego	Arizona	866-638-9753
San Diego	California	866-638-9753
San Francisco	California	650-876-2872
San Francisco	Wyoming	650-876-2872
San Juan	Puerto Rico	787-253-7880
San Juan	U.S. Virgin Islands	787-253-7880
```

## Data source

The station names, addresses, phones, fax numbers and coverage notes come from the CDC [Port Health Station Contact List](https://www.cdc.gov/port-health/stations/port-health-station-contact-list.html), dated May 15, 2024 and fetched on November 5, 2026. Address lines were joined with commas, and each state, territory or area a station covers was given its own row. The substance of the CDC list was not changed. Check the CDC page before you ship anything or call about a sick traveler, because contacts can move.

Credit lines:

* Source: CDC. Data from the CDC Port Health Station Contact List (https://www.cdc.gov/port-health/stations/port-health-station-contact-list.html), dated May 15, 2024, fetched November 5, 2026. This information is free on the CDC website at https://www.cdc.gov/port-health/stations/port-health-station-contact-list.html. CDC does not endorse this API or the people who run it. Changes: one row for each state, territory or area a station covers, and address lines joined with commas; no change to the substance. US federal government works are in the public domain (17 U.S.C. 105); see CDC Use of Agency Materials (https://www.cdc.gov/other/agencymaterials.html).

[Back to the API list](../../README.md)
