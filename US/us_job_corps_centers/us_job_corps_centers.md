# US Job Corps Centers

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 122](https://img.shields.io/badge/records-122-lightgrey) ![updated 2026-03-17](https://img.shields.io/badge/updated-2026--03--17-orange) ![license cite CareerOneStop](https://img.shields.io/badge/license-cite%20CareerOneStop-green)

Job Corps centers in the United States and Puerto Rico, from the CareerOneStop Job Corps Center Finder, which is sponsored by the US Department of Labor. There are 122 centers in all 50 states, DC and Puerto Rico. California, Kentucky and New York have 7 each and Oregon has 6. Each row has the center name, street address, city, state, ZIP code, phone, website, the date CareerOneStop last updated the listing and a map point. Filter by state, city, ZIP, name, update date or a coordinate range. All filters are optional.

> **Data age.** The list was fetched on 2026-11-01. CareerOneStop last updated 92 listings on 2025-12-10 and 30 on 2026-03-17. Check the center website before you visit.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/us_job_corps_centers` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 0 fields. Optional: 12 fields. Disallowed: 2 fields.

No filter is required, so a call with no parameters returns all 122 centers. Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `name` | Optional | Yes | No | Center name as CareerOneStop shows it, such as `*Civilian Conservation*` (20 rows) or `Atterbury*`. One row per name. |
| `address` | Optional | Yes | No | Street address as listed. It can include a suite, building or P O Box, such as `*Highway*` (13) or `*P O Box*` (17). |
| `city` | Optional | Yes | No | City as CareerOneStop lists it, such as `Los Angeles` or `St. Louis`. |
| `state` | Optional | Yes | No | 2 letter state code. 52 codes, including `DC` and `PR` (Puerto Rico). Most common: `CA` (7), `KY` (7), `NY` (7), `OR` (6), then `NC`, `PA`, `TX` and `WA` (4 each). |
| `state_name` | Optional | Yes | No | Full state name, such as `New York` (7) or `Puerto Rico` (2). |
| `zip` | Optional | Yes | No | ZIP code as listed, 5 digits or ZIP+4 (29 rows), stored as text so leading zeros stay, such as `01022` or `0*` (13). |
| `phone` | Optional | Yes | No | Main center phone number written as `123-456-7890`, such as `606*`. |
| `website` | Optional | Yes | No | Center website (https), such as `*atterbury*`. Guthrie and Tulsa share one website. |
| `last_updated` | Optional | No | Yes | Date CareerOneStop last updated the listing: 2025-12-10 (92) or 2026-03-17 (30). Returned as a date and time, such as `2025-12-10T00:00:00+00:00`. Use an exact date or a range, for example `last_updated=2026-01-01..2026-12-31`. |
| `lat` | Optional | No | Yes | Latitude in decimal degrees, from 18.438398 to 61.588535. Example: `lat=40..42`. |
| `long` | Optional | No | Yes | Longitude in decimal degrees, from -157.719365 to -66.594111. Example: `long=-80..-70`. |
| `geocode_precision` | Optional | No | No | How the map point was found: `census_address` (101, a US Census Geocoder street match), `zcta_centroid` (17, the center of the ZIP code area) or `place_centroid` (4, the center of the city). The last two come from the 2024 Census Gazetteer and were used when the street did not match. |
| `census_matched_address` | Disallowed | No | No | The address the US Census Geocoder matched, in upper case. Blank for 21 rows with no street match. |
| `source_url` | Disallowed | No | No | CareerOneStop finder page the row came from (https). |

* Parameters are combined with AND. Text matches ignore case, so `state=ky` works the same as `state=KY`. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.
* A filter on `census_matched_address` or `source_url` returns HTTP 400 with `Request attempts to filter on disallowed parameters: source_url`.
* `last_updated`, `lat` and `long` take an exact value or a range, not a wildcard. `last_updated=2026*` returns HTTP 400 with `Value '2026*' is not a datetime`.

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `name` | string | Center name. |
| `address` | string | Street address as listed. |
| `city` | string | City. |
| `state` | string | 2 letter state code. |
| `state_name` | string | Full state name. |
| `zip` | string | ZIP code, 5 digits or ZIP+4. |
| `phone` | string | Main center phone number. |
| `website` | string | Center website (https). |
| `last_updated` | string | Date and time CareerOneStop last updated the listing, such as `2025-12-10T00:00:00+00:00`. |
| `lat` | number | Latitude in decimal degrees, 6 decimals. |
| `long` | number | Longitude in decimal degrees, 6 decimals. |
| `geocode_precision` | string | `census_address`, `zcta_centroid` or `place_centroid`. |
| `census_matched_address` | string | Address the US Census Geocoder matched. Null for 21 rows. |
| `source_url` | string | CareerOneStop finder page the row came from. |

Notes:

* 21 map points are not street matches. 17 are the center of the ZIP code area and 4 (Roswell, Talking Leaves, Ramey and Gary) are the center of the city, so they can be a few miles off.
* There is no official center code in the source, so use `__id` as the key.

## Examples

Range and wildcard together. Civilian Conservation Centers between latitude 36 and 38. This call returns 3 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/us_job_corps_centers" --data-urlencode "lat=36..38" --data-urlencode "name=*Civilian Conservation*"
```

```json
[
  {
    "__id": 56,
    "name": "Mingo Job Corps Civilian Conservation Center",
    "address": "4253 State Highway T",
    "city": "Puxico",
    "state": "MO",
    "state_name": "Missouri",
    "zip": "63960-9585",
    "phone": "573-222-3537",
    "website": "https://mingo.jobcorps.gov/",
    "last_updated": "2025-12-10T00:00:00+00:00",
    "lat": 36.935436,
    "long": -90.200672,
    "geocode_precision": "census_address",
    ...
  },
  ...
]
```

The same call as a table:

```bash
curl -sG "https://api.jael.ee/JLEE/us_job_corps_centers" --data-urlencode "lat=36..38" --data-urlencode "name=*Civilian Conservation*" | jq -r '.[] | [.name, .city, .state] | @tsv'
```

```text
Mingo Job Corps Civilian Conservation Center	Puxico	MO
Jacobs Creek Job Corps Civilian Conservation Center	Bristol	TN
Flatwoods Job Corps Civilian Conservation Center	Coeburn	VA
```

One state. Centers in Kentucky. This call returns 7 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/us_job_corps_centers" --data-urlencode "state=KY" | jq -r '.[] | [.name, .city, .zip] | @tsv'
```

```text
Carl D. Perkins Job Corps Center	Prestonsburg	41653
Earle C Clements Job Corps Center	Morganfield	42437-6608
Frenchburg Job Corps Center	Frenchburg	40322
Great Onyx Job Corps Center	Mammoth Cave	42259
Muhlenberg Job Corps Center	Greenville	42345
Pine Knot Job Corps Center	Pine Knot	42635
Whitney M. Young Job Corps Center	Simpsonville	40067
```

Date range. Listings CareerOneStop updated in 2026. This call returns 30 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/us_job_corps_centers" --data-urlencode "last_updated=2026-01-01..2026-12-31" | jq length
```

ZIP wildcard. Centers with a ZIP code that starts with `0`, in New England, New Jersey and Puerto Rico. This call returns 13 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/us_job_corps_centers" --data-urlencode "zip=0*" | jq length
```

## Data source

The center list comes from the CareerOneStop Job Corps Center Finder, which is sponsored by the US Department of Labor. It was fetched on 2026-11-01. CareerOneStop allows sharing and reuse of its data when CareerOneStop is cited as the source. The location text was split into address, city, state and ZIP, and dates were reformatted. Map points come from the US Census Geocoder and the 2024 Census ZIP code (ZCTA) and place Gazetteer files, which are in the public domain.

Credit lines:

* Source: CareerOneStop, sponsored by the U.S. Department of Labor, Job Corps Center Finder (https://www.careeronestop.org/LocalHelp/EmploymentAndTraining/find-job-corps-centers.aspx), fetched 1 November 2026. Reuse terms: https://www.careeronestop.org/Veterans/Help/cite-this-website.aspx
* Coordinates: US Census Geocoder (https://geocoding.geo.census.gov) and 2024 Census Gazetteer files, public domain.
* Not endorsed by CareerOneStop or the U.S. Department of Labor.

[Back to the API list](../../README.md)
