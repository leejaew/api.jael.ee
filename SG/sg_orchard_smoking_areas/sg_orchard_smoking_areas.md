# Singapore Orchard Road Designated Smoking Areas

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 52](https://img.shields.io/badge/records-52-lightgrey) ![updated 2026-09-19](https://img.shields.io/badge/updated-2026--09--19-orange) ![license SODL v1.0](https://img.shields.io/badge/license-SODL%20v1.0-green)

The 52 Designated Smoking Areas that the National Environment Agency (NEA) lists in and around Orchard Road, Singapore, from data.gov.sg. Each row has the NEA id, the building or place, where the area is at the building, a link to NEA's photo, the URA planning area and the map point. Filter by building name, spot, planning area, date updated or a coordinate range. No filter is required, so a call with no parameters returns all 52.

The 52 areas sit at 35 buildings or places, in the planning areas Orchard (49), Newton (2) and Museum (1).

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/sg_orchard_smoking_areas` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: none. Optional: 7 fields. Disallowed: 2 fields.

Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `smoking_area_id` | Optional | No | Yes | NEA object id of the smoking area, a whole number from 17921 to 17972. One per row. Example: `smoking_area_id=17921..17930` (10). |
| `building_name` | Optional | Yes | No | Building or place the smoking area belongs to, such as `Ngee Ann City` (4) or `*Hotel*` (11). NEA's ALL CAPS names were changed to title case. A building can have more than one smoking area. |
| `location_description` | Optional | Yes | No | Where the area is at the building, as NEA writes it: `Back of building` (17), `Side of building` (17), `Front of building` (11), and 7 others with 1 row each, such as `Tower A Level 7 open carpark`. |
| `planning_area` | Optional | Yes | No | URA Master Plan 2019 planning area the point falls in: `Orchard` (49), `Newton` (2) or `Museum` (1). |
| `latitude` | Optional | No | Yes | Latitude in decimal degrees (WGS 84), from 1.299687 to 1.309436. Example: `latitude=1.30..1.305` (25). |
| `longitude` | Optional | No | Yes | Longitude in decimal degrees (WGS 84), from 103.823619 to 103.844949. Example: `longitude=103.83..103.835` (18). |
| `record_updated` | Optional | Yes | No | Date NEA last updated the row, as `YYYY-MM-DD` text. Use the full date, or a prefix wildcard such as `2026*` (3) for a year. Values: `2025-03-05` (44), `2025-11-03` (3), `2025-10-10` (2), `2026-03-17` (2) and `2026-09-17` (1). |
| `photo_url` | Disallowed | No | No | Link to NEA's photo of the smoking area on www.nea.gov.sg. Open the link to see it. The image itself is not stored here. |
| `source_url` | Disallowed | No | No | Source dataset page on data.gov.sg. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.
* `record_updated` is text, so a date range such as `2025-11-01..2026-12-31` does not work and returns an empty list. Use a prefix wildcard instead.

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `smoking_area_id` | integer | NEA object id of the smoking area. |
| `building_name` | string | Building or place. |
| `location_description` | string | Where the area is at the building. |
| `photo_url` | string | Link to NEA's photo of the area. |
| `planning_area` | string | URA Master Plan 2019 planning area. |
| `latitude` | number | Latitude in decimal degrees (WGS 84). |
| `longitude` | number | Longitude in decimal degrees (WGS 84). |
| `record_updated` | string | Date NEA last updated the row, `YYYY-MM-DD`. |
| `source_url` | string | Source dataset page on data.gov.sg. |

## Examples

A prefix wildcard on a date. Areas NEA updated in 2026. This call returns 3 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/sg_orchard_smoking_areas" --data-urlencode "record_updated=2026*"
```

Response (3 rows, first row shown):

```json
[
  {
    "__id": 1,
    "smoking_area_id": 17921,
    "building_name": "Vibe Hotel Singapore Orchard",
    "location_description": "Front of building",
    "photo_url": "https://www.nea.gov.sg/images/default-source/our-serivces/smoking-prohibition/dsa/vibe-hotel-singapore-orchard-dsa.jpg",
    "planning_area": "Newton",
    "latitude": 1.307976,
    "longitude": 103.83544,
    "record_updated": "2026-09-17",
    "source_url": "https://data.gov.sg/datasets/d_d0fa8f07ef80ab23feaa3b870323bf27/view"
  },
  ...
]
```

A name wildcard. Smoking areas at hotels. This call returns 11 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/sg_orchard_smoking_areas" --data-urlencode "building_name=*Hotel*" | jq -r '.[] | [.building_name, .location_description] | @tsv'
```

```text
Vibe Hotel Singapore Orchard	Front of building
Goodwood Park Hotel	Front of building
York Hotel	Back of building
Four Seasons Hotel	Back of building
Four Seasons Hotel	Front of building
Goodwood Park Hotel	Side of building
York Hotel	Front of building
Goodwood Park Hotel	Back of building
Artyzen Hotel	Back of building
Concorde Hotel and Shopping Mall	Along Kramat Lane
Concorde Hotel and Shopping Mall	Back of building
```

A latitude and longitude range. Areas in a small box around Ngee Ann City and Lucky Plaza. This call returns 5 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/sg_orchard_smoking_areas" --data-urlencode "latitude=1.30..1.305" --data-urlencode "longitude=103.83..103.835" | jq -r '.[] | [.building_name, .location_description] | @tsv'
```

```text
Ngee Ann City	Side of building
Nutmeg Road	Behind Lucky Plaza
Wheelock Place	Back of building
Lucky Plaza	Side of building
Ngee Ann City	Tower B Level 7 open carpark
```

## Data source

Smoking areas come from Designated Smoking Areas (National Environment Agency), on data.gov.sg ([dataset page](https://data.gov.sg/datasets/d_d0fa8f07ef80ab23feaa3b870323bf27/view)), last updated September 19, 2026. Planning areas come from the URA Master Plan 2019 Planning Area Boundary (No Sea) on data.gov.sg. License: [Singapore Open Data Licence version 1.0](https://data.gov.sg/open-data-licence). It allows free use, including commercial use, as long as you credit the source and do not suggest the agency backs you.

Credit line: Contains information from Designated Smoking Areas (National Environment Agency, last updated 19 September 2026) accessed on 1 November 2026 from data.gov.sg (https://data.gov.sg/datasets/d_d0fa8f07ef80ab23feaa3b870323bf27/view), and from Master Plan 2019 Planning Area Boundary (No Sea) (Urban Redevelopment Authority) accessed on 1 November 2026 from data.gov.sg, which are made available under the terms of the Singapore Open Data Licence version 1.0 (https://data.gov.sg/open-data-licence).

Changes made: building names were changed to title case and planning areas were added from the map point. Photos belong to NEA and are linked, not copied. This API is not endorsed by NEA. Smoking areas can be added or removed, so check the signs on site.

[Back to the API list](../../README.md)
