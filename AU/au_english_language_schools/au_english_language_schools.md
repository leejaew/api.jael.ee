# Australian English Language Schools

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 811](https://img.shields.io/badge/records-811-lightgrey) ![updated 2026-11-02](https://img.shields.io/badge/updated-2026--11--02-orange) ![license CC BY 2.5 AU](https://img.shields.io/badge/license-CC%20BY%202.5%20AU-green)

All 811 Australian locations where 436 CRICOS registered providers deliver ELICOS (English Language Intensive Courses for Overseas Students) to student visa holders, from the CRICOS export as at 2 November 2026, with each location's address, its ELICOS courses, course lengths and tuition fees, and a map point. Filter by the required `state`, then by provider, suburb, postcode, course name, fee or a latitude and longitude range.

Rows by state: NSW (267), VIC (247), QLD (165), SA (56), WA (43), ACT (16), TAS (12) and NT (5). Private providers: 742. Government providers: 69.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/au_english_language_schools` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`state`). Optional: 16 fields. Disallowed: 5 fields.

`state` must be in every call. Use `state=*` to list all 811 locations. Disallowed means the field is returned in the output but cannot be used as a filter. Without `state` the API returns HTTP 400, and a filter on a disallowed field also returns HTTP 400.

Course counts, weeks, fees and coordinates take ranges.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `state` | Required | Yes | No | State or territory code: `NSW` (267), `VIC` (247), `QLD` (165), `SA` (56), `WA` (43), `ACT` (16), `TAS` (12) or `NT` (5). Wildcards work, so `state=*` lists all 811 rows. |
| `provider_code` | Optional | No | No | Exact CRICOS provider code, such as `00001K` (Canberra Institute of Technology, 2 locations). A provider can have many locations. |
| `provider_name` | Optional | Yes | No | Registered provider (institution) name, such as `*University*` (50) or `*TAFE*` (5). Wildcards work. |
| `trading_name` | Optional | Yes | No | Provider's trading name, such as `*Navitas*` (4). Wildcards work. Blank on 144 rows where CRICOS lists none. |
| `provider_type` | Optional | No | No | Exact: `Private` (742) or `Government` (69). |
| `location_name` | Optional | Yes | No | Location name as the provider registered it, often a campus name or an address, such as `*Campus*` (182). Wildcards work. |
| `location_type` | Optional | No | No | Exact: `Location owned and operated by provider` (742), `Arrangement with Non Registered Provider` (48) or `Arrangement with other registered provider` (21). |
| `suburb` | Optional | Yes | No | Suburb or city, such as `Melbourne` (156), `Sydney` (90) or `Brisbane*` (47 in QLD). Wildcards work. All capital names in CRICOS were changed to title case. |
| `postcode` | Optional | Yes | No | 4 digit postcode, such as `2000` (120 in NSW), `3000` (154 in VIC) or `4217` (14 in QLD). Wildcards work, such as `20*`. |
| `elicos_courses` | Optional | No | Yes | Number of current ELICOS courses at this location, from 1 to 17. Use a range such as `5..17` (141). |
| `course_names` | Optional | Yes | No | Names of the ELICOS courses at this location, separated by semicolons, such as `*IELTS*` (250) or `*Academic*` (411). Wildcards work. |
| `min_weeks` | Optional | No | Yes | Shortest course duration at this location in weeks, from 1 to 118. Use a range. |
| `max_weeks` | Optional | No | Yes | Longest course duration at this location in weeks, from 1 to 280. Use a range such as `52..280` (611). |
| `min_tuition_aud` | Optional | No | Yes | Lowest course tuition fee at this location in Australian dollars as listed in CRICOS, from 0 to 50592. Blank on 10 rows. Use a range such as `0..5000` (209). |
| `max_tuition_aud` | Optional | No | Yes | Highest course tuition fee at this location in Australian dollars, from 0 to 56000. Blank on 10 rows. Use a range such as `20000..60000` (362). |
| `latitude` | Optional | No | Yes | Latitude in decimal degrees (WGS 84), from -42.9036014 to -12.3737641. Use a range, for example `latitude=-34.0..-33.8&longitude=151.1..151.3` for central Sydney (192). |
| `longitude` | Optional | No | Yes | Longitude in decimal degrees (WGS 84), from 115.7694682 to 153.6162912. Use a range with latitude to search a box. |
| `website` | Disallowed | No | No | Provider website from CRICOS, written with https. Blank on 268 rows. |
| `address` | Disallowed | No | No | Street address of the location from CRICOS, with lines joined by commas. |
| `geocode_quality` | Disallowed | No | No | How exact the map point is, best first: `address` (465), `place` (3), `street` (286), `suburb_centroid` (54) or `postcode_centroid` (3). Points are approximate. |
| `source` | Disallowed | No | No | The CRICOS export file on data.gov.au the row comes from. |
| `updated` | Disallowed | No | No | Date of the CRICOS snapshot, `YYYY-MM-DD` (`2026-11-02`). Text. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.
* A call without `state` returns HTTP 400 with `Your request is missing the following required parameters: state. You did not supply any.`
* A filter on a disallowed field returns HTTP 400.

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `provider_code` | string | CRICOS provider code. |
| `provider_name` | string | Registered institution name. |
| `trading_name` | string | Trading name. Null on 144 rows. |
| `provider_type` | string | `Private` or `Government`. |
| `website` | string | Provider website. Null on 268 rows. |
| `location_name` | string | Campus or location name. |
| `location_type` | string | How the location is operated. |
| `address` | string | Street address, lines joined by commas. |
| `suburb` | string | Suburb or city in title case. |
| `state` | string | State or territory code. |
| `postcode` | string | 4 digit postcode. |
| `elicos_courses` | number | Count of current ELICOS courses at this location. |
| `course_names` | string | Course names joined with semicolons. |
| `min_weeks` | number | Shortest course length in weeks. |
| `max_weeks` | number | Longest course length in weeks. |
| `min_tuition_aud` | number | Lowest tuition fee in AUD. Null on 10 rows. |
| `max_tuition_aud` | number | Highest tuition fee in AUD. Null on 10 rows. |
| `latitude` | number | Latitude (WGS 84). |
| `longitude` | number | Longitude (WGS 84). |
| `geocode_quality` | string | How exact the map point is. |
| `source` | string | Link to the CRICOS export. |
| `updated` | string | Snapshot date, `YYYY-MM-DD`. |

## Examples

Sydney CBD postcode. This call returns 90 rows (one shown):

```bash
curl -sG "https://api.jael.ee/JLEE/au_english_language_schools" --data-urlencode "state=NSW" --data-urlencode "postcode=2000" --data-urlencode "suburb=Sydney"
```

```json
[
  {
    "__id": 17,
    "provider_code": "04100H",
    "provider_name": "A GRADE EDUCATION AND TRAINING PTY LIMITED",
    "trading_name": null,
    "provider_type": "Private",
    "website": null,
    "location_name": "Kent Street",
    "location_type": "Location owned and operated by provider",
    "address": "Suite 502 Level 5 545-553 Kent Street",
    "suburb": "Sydney",
    "state": "NSW",
    "postcode": "2000",
    "elicos_courses": 1.0,
    "course_names": "ELICOS General English",
    "min_weeks": 60.0,
    "max_weeks": 60.0,
    "min_tuition_aud": 15000.0,
    "max_tuition_aud": 15000.0,
    "latitude": -33.8698439,
    "longitude": 151.2082848,
    "geocode_quality": "suburb_centroid",
    "source": "https://data.gov.au/data/dataset/e5ae7059-bfa8-4fa4-a5c0-c13cf3520193/resource/11f8b01e-5a52-4e19-a3b8-7fea8a0430bf/download/cricos-providers-courses-and-locations.zip",
    "updated": "2026-11-02"
  }
]
```

IELTS courses. This call returns 250 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/au_english_language_schools" --data-urlencode "state=*" --data-urlencode "course_names=*IELTS*" | jq length
```

Government providers. This call returns 69 rows:

```bash
curl -s "https://api.jael.ee/JLEE/au_english_language_schools?state=*&provider_type=Government" | jq length
```

Map box over central Sydney. This call returns 192 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/au_english_language_schools" --data-urlencode "state=*" --data-urlencode "latitude=-34.0..-33.8" --data-urlencode "longitude=151.1..151.3" | jq length
```

## Data source

The locations come from the Australian Government Department of Education, Commonwealth Register of Institutions and Courses for Overseas Students (CRICOS), [CRICOS Providers, Courses and Locations](https://data.gov.au/data/dataset/cricos) export as at 2 November 2026, fetched on 5 November 2026. Licensed under the [Creative Commons Attribution 2.5 Australia (CC BY 2.5 AU)](https://creativecommons.org/licenses/by/2.5/au/) license, which lets anyone copy, change and use the data, including for commercial use, as long as the source is credited.

Changes: current ELICOS courses (field of education 091501, Non AQF Award, not expired) were kept and joined to their providers and course locations, one row per location with course counts, names, lengths and fee ranges; address lines were joined with commas; all capital suburb names were changed to title case; websites were written with https; map points were added. For the latest data visit https://cricos.education.gov.au/. The data is not guaranteed to be complete or accurate.

Credit lines:

* Source: Australian Government Department of Education, Commonwealth Register of Institutions and Courses for Overseas Students (CRICOS), CRICOS Providers, Courses and Locations export as at 2 November 2026 (https://data.gov.au/data/dataset/cricos), fetched 5 November 2026. Licensed under the Creative Commons Attribution 2.5 Australia license (https://creativecommons.org/licenses/by/2.5/au/).
* Map points from OpenStreetMap contributors via Nominatim (ODbL, https://www.openstreetmap.org/copyright).
* Not endorsed by the Department of Education.

[Back to the API list](../../README.md)
