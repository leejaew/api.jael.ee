# Singapore ABC Waters Projects

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 75](https://img.shields.io/badge/records-75-lightgrey) ![updated 2023-09-28](https://img.shields.io/badge/updated-2023--09--28-orange) ![license SODL v1.0](https://img.shields.io/badge/license-SODL%20v1.0-green)

The 75 completed Active, Beautiful, Clean Waters (ABC Waters) projects that PUB, Singapore's national water agency, lists on data.gov.sg. The sites include rivers, canals, reservoirs, ponds and rain gardens. Each row has the PUB id, project name, URA planning area and region, the share of the site in that area, site area and outline length, the number of shapes in the site, and a center map point. Filter by name, planning area, region, site size or a coordinate range. No filter is required, so a call with no parameters returns all 75.

Rows by region: Central 24, West 15, East 15, North 11 and North-East 10. The site areas run from 73 to 678,077 square meters.

> **Data age.** data.gov.sg shows the dataset as updated on 2026-11-01, but PUB dates 73 rows 2023-09-28 and 2 rows 2023-05-03. Projects finished after that may be missing.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/sg_abc_waters` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: none. Optional: 9 fields. Disallowed: 4 fields.

Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `project_id` | Optional | No | Yes | PUB object id of the project, a whole number from 3201 to 3275. One per row. Example: `project_id=3201..3210` (10). |
| `project_title` | Optional | Yes | No | Project name as PUB writes it, such as `*Kallang River*` (7) or `*Rain Garden*` (9). |
| `planning_area` | Optional | Yes | No | URA Master Plan 2019 planning area that holds the largest part of the site. 27 areas are used, such as `Bedok` (9), `Kallang` (7), `Yishun` (5), `Jurong East` (4), `Sengkang` (4), `Pasir Ris` (4), `Clementi` (4), `Bukit Merah` (3), `Bishan` (3), `Bukit Batok` (3), `Hougang` (3) and `Queenstown` (3). Long waterways can cross into other areas too. |
| `region` | Optional | Yes | No | URA region of that planning area: `Central Region` (24), `West Region` (15), `East Region` (15), `North Region` (11) or `North-East Region` (10). |
| `area_sq_m` | Optional | No | Yes | Site area in square meters as PUB gives it, rounded, from 73 to 678077. Example: `area_sq_m=50000..1000000` (21). |
| `perimeter_m` | Optional | No | Yes | Length of the site outline in meters as PUB gives it, rounded, from 39 to 8540. Example: `perimeter_m=1000..9000` (37). |
| `latitude` | Optional | No | Yes | Latitude of the site center point in decimal degrees (WGS 84), from 1.268661 to 1.446775. Example: `latitude=1.34..1.36` (12). |
| `longitude` | Optional | No | Yes | Longitude of the site center point in decimal degrees (WGS 84), from 103.678486 to 103.955482. Example: `longitude=103.84..103.86` (6). |
| `centroid_on_site` | Optional | No | No | Whether the center point lies inside the site outline: `yes` (53) or `no` (22). For long or curved waterways the center point can fall just beside the site. Exact match only. |
| `planning_area_share_pct` | Disallowed | No | No | Percent of the site inside `planning_area`, from 50 to 100. It is 100 for 64 rows. |
| `site_parts` | Disallowed | No | No | Number of separate shapes that make up the site: `1` (69) or `2` (6). |
| `record_updated` | Disallowed | No | No | Date PUB last updated the row, as `YYYY-MM-DD`: `2023-09-28` (73) or `2023-05-03` (2). |
| `source_url` | Disallowed | No | No | Source dataset page on data.gov.sg. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `project_id` | integer | PUB object id of the project. |
| `project_title` | string | Project name as PUB writes it. |
| `planning_area` | string | URA Master Plan 2019 planning area that holds the largest part of the site. |
| `planning_area_share_pct` | integer | Percent of the site inside that planning area. |
| `region` | string | URA region. |
| `area_sq_m` | integer | Site area in square meters, rounded. |
| `perimeter_m` | integer | Length of the site outline in meters, rounded. |
| `site_parts` | integer | Number of separate shapes in the site. |
| `latitude` | number | Latitude of the site center point (WGS 84). |
| `longitude` | number | Longitude of the site center point (WGS 84). |
| `centroid_on_site` | string | `yes` if the center point lies inside the site outline, else `no`. |
| `record_updated` | string | Date PUB last updated the row, `YYYY-MM-DD`. |
| `source_url` | string | Source dataset page on data.gov.sg. |

## Examples

A range on site area. Large projects in the East Region. This call returns 5 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/sg_abc_waters" --data-urlencode "region=East Region" --data-urlencode "area_sq_m=50000..1000000"
```

Response (5 rows, first row shown):

```json
[
  {
    "__id": 9,
    "project_id": 3209,
    "project_title": "Sungei Serangoon - Lorong Halus Wetlands",
    "planning_area": "Pasir Ris",
    "planning_area_share_pct": 99,
    "region": "East Region",
    "area_sq_m": 95931,
    "perimeter_m": 1629,
    "site_parts": 1,
    "latitude": 1.395489,
    "longitude": 103.924305,
    "centroid_on_site": "yes",
    "record_updated": "2023-09-28",
    "source_url": "https://data.gov.sg/datasets/d_b5f239cc954bb7920245a7b8ad95dde8/view"
  },
  ...
]
```

A name wildcard. Every project along the Kallang River. This call returns 7 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/sg_abc_waters" --data-urlencode "project_title=*Kallang River*" | jq -r '.[] | [.project_title, .planning_area, .area_sq_m, .centroid_on_site] | @tsv'
```

```text
Kallang River @ River Vista	Kallang	8431	yes
Kallang River @ Bishan-Ang Mo Kio Park	Bishan	678077	yes
Kallang River @ Potong Pasir	Toa Payoh	29749	yes
Sungei Whampoa (Blk 4B St George's Lane to Kallang River)	Kallang	32789	no
Kallang River - Tributary Drain along Lorong 8 Toa Payoh	Toa Payoh	8393	yes
Kallang River (Upper Boon Keng Road - Sims Ave)	Kallang	32501	yes
Kallang River (Bishan Road to Braddell Road)	Bishan	86618	no
```

## Data source

Project names, site shapes, areas and outline lengths come from ABC Waters Completed (PUB, Singapore's national water agency), on data.gov.sg ([dataset page](https://data.gov.sg/datasets/d_b5f239cc954bb7920245a7b8ad95dde8/view)), shown as last updated November 1, 2026. Planning areas and regions come from the URA Master Plan 2019 Planning Area Boundary (No Sea) on data.gov.sg. License: [Singapore Open Data Licence version 1.0](https://data.gov.sg/open-data-licence). It allows free use, including commercial use, as long as you credit the source and do not suggest the agency backs you.

Credit line: Contains information from ABC Waters Completed (PUB, last updated 1 November 2026) accessed on 1 November 2026 from data.gov.sg (https://data.gov.sg/datasets/d_b5f239cc954bb7920245a7b8ad95dde8/view), and from Master Plan 2019 Planning Area Boundary (No Sea) (Urban Redevelopment Authority) accessed on 1 November 2026 from data.gov.sg, which are made available under the terms of the Singapore Open Data Licence version 1.0 (https://data.gov.sg/open-data-licence).

Changes made: center points, planning areas and the share of each site in its planning area were worked out from the site shapes. The shapes themselves are not returned. This API is not endorsed by PUB. The rows date from 2023, so check PUB before you rely on a row.

[Back to the API list](../../README.md)
