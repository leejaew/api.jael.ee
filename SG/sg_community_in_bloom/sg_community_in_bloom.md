# Singapore Community in Bloom Gardens

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 1772](https://img.shields.io/badge/records-1772-lightgrey) ![updated 2024-11-21](https://img.shields.io/badge/updated-2024--11--21-orange) ![license SODL v1.0](https://img.shields.io/badge/license-SODL%20v1.0-green)

The 1,772 community gardens in the Community in Bloom program of the National Parks Board (NParks), from data.gov.sg. Gardens are run by residents, schools, firms and other groups. Each row has the NParks garden id, garden name, URA planning area and region, and a map point that NParks gives as a rough access point to the garden. Filter by the required `planning_area`, then by name, region, garden id or a coordinate range.

Rows by region: Central 481, West 426, North-East 361, East 328 and North 176.

> **Data age.** NParks last updated the source on 2024-11-21. Gardens start and close over time, so some rows may be out of date.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/sg_community_in_bloom` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`planning_area`). Optional: 5 fields. Disallowed: 2 fields.

`planning_area` must be in every call. Use `planning_area=*` to list all 1,772 gardens. Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `planning_area` | Required | Yes | No | URA Master Plan 2019 planning area the garden point falls in. 46 areas are used, such as `Bedok` (161), `Jurong West` (133), `Tampines` (109), `Hougang` (96), `Sengkang` (80), `Woodlands` (74), `Bukit Batok` (66), `Serangoon` (65), `Queenstown` (63), `Bukit Panjang` (61), `Yishun` (60) and `Ang Mo Kio` (60). Use `planning_area=*` for all 1,772. |
| `garden_id` | Optional | No | Yes | NParks garden id, a whole number from 1 to 1772. One per row. Example: `garden_id=1..100` (100). |
| `garden_name` | Optional | Yes | No | Garden name as NParks writes it, such as `*Primary School*` (113) or `*Residents' Committee*` (326). Case varies in the source, and some names are ALL CAPS, so use wildcards. Extra spaces were removed. |
| `region` | Optional | Yes | No | URA region of the planning area: `Central Region` (481), `West Region` (426), `North-East Region` (361), `East Region` (328) or `North Region` (176). |
| `latitude` | Optional | No | Yes | Latitude of the garden access point in decimal degrees (WGS 84), from 1.247904 to 1.46195. Example: `latitude=1.34..1.36` (382). Some gardens share a point. |
| `longitude` | Optional | No | Yes | Longitude of the garden access point in decimal degrees (WGS 84), from 103.633617 to 103.99005. Example: `longitude=103.84..103.86` (193). |
| `source_updated` | Disallowed | No | No | Date NParks last updated the dataset on data.gov.sg, as `YYYY-MM-DD`: `2024-11-21` on all 1,772 rows. |
| `source_url` | Disallowed | No | No | Source dataset page on data.gov.sg. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.
* A call without `planning_area` returns a 400 error: `Your request is missing the following required parameters: planning_area. You did not supply any.`

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `garden_id` | integer | NParks garden id. |
| `garden_name` | string | Garden name as NParks writes it. |
| `planning_area` | string | URA Master Plan 2019 planning area. |
| `region` | string | URA region. |
| `latitude` | number | Latitude of the garden access point (WGS 84). |
| `longitude` | number | Longitude of the garden access point (WGS 84). |
| `source_updated` | string | Date NParks last updated the dataset, `YYYY-MM-DD`. |
| `source_url` | string | Source dataset page on data.gov.sg. |

## Examples

A name wildcard. School gardens in Bedok. This call returns 7 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/sg_community_in_bloom" --data-urlencode "planning_area=Bedok" --data-urlencode "garden_name=*Primary School*"
```

Response (7 rows, first row shown):

```json
[
  {
    "__id": 766,
    "garden_id": 766,
    "garden_name": "YU NENG PRIMARY SCHOOL",
    "planning_area": "Bedok",
    "region": "East Region",
    "latitude": 1.333904,
    "longitude": 103.932034,
    "source_updated": "2024-11-21",
    "source_url": "https://data.gov.sg/datasets/d_f91a8b057cfb2bebf2e531ad8061e1c1/view"
  },
  ...
]
```

A latitude and longitude range in all planning areas. Gardens in a small box around Marine Parade and Joo Chiat. This call returns 10 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/sg_community_in_bloom" --data-urlencode "planning_area=*" --data-urlencode "latitude=1.30..1.31" --data-urlencode "longitude=103.90..103.91" | jq -r '.[] | [.garden_id, .garden_name, .planning_area] | @tsv'
```

```text
139	Odyssey The Global Preschool	Bedok
140	Pat's Schoolhouse Pte Ltd (Katong)	Marine Parade
142	D'Marine Apartments	Marine Parade
144	Joo Chiat NC 1 @ Marine Parade (The Breeze Villas)	Marine Parade
155	Marine Drive Garden Residents' Committee	Marine Parade
156	WhizKid Preschool	Marine Parade
812	Joo Chiat Community Centre	Marine Parade
826	Soon Service Auto Centre Pte Ltd - Shell Dunman Service Station	Geylang
1097	Chinese Swimming Club	Marine Parade
1535	Tung Ling Preschool Education	Marine Parade
```

## Data source

Garden ids, names and points come from Community in Bloom (CIB) (National Parks Board), on data.gov.sg ([dataset page](https://data.gov.sg/datasets/d_f91a8b057cfb2bebf2e531ad8061e1c1/view)), last updated November 21, 2024. Planning areas and regions come from the URA Master Plan 2019 Planning Area Boundary (No Sea) on data.gov.sg. License: [Singapore Open Data Licence version 1.0](https://data.gov.sg/open-data-licence). It allows free use, including commercial use, as long as you credit the source and do not suggest the agency backs you.

Credit line: Contains information from Community in Bloom (CIB) (National Parks Board, last updated 21 November 2024) accessed on 1 November 2026 from data.gov.sg (https://data.gov.sg/datasets/d_f91a8b057cfb2bebf2e531ad8061e1c1/view), and from Master Plan 2019 Planning Area Boundary (No Sea) (Urban Redevelopment Authority) accessed on 1 November 2026 from data.gov.sg, which are made available under the terms of the Singapore Open Data Licence version 1.0 (https://data.gov.sg/open-data-licence).

Changes made: garden names were tidied and planning areas were added from the map point. This API is not endorsed by NParks. Some gardens are on school or private grounds, so ask before you visit.

[Back to the API list](../../README.md)
