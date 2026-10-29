# UAE Higher Education Institutions

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 160](https://img.shields.io/badge/records-160-lightgrey) ![updated 2026-10-29](https://img.shields.io/badge/updated-2026--10--29-orange) ![license public register facts](https://img.shields.io/badge/license-public%20register%20facts-green)

Every college and university on the UAE Commission for Academic Accreditation (CAA) register as of 2026-10-29, 160 institutions in all seven emirates. Each row has the name in English (upper case, as CAA prints it) and in Arabic, the emirate of the main campus, every emirate CAA lists for it, the city, the licence status, the website and a map point. Licence status is `active` for 104 institutions, `inactive` for 31, `licensure_revoked` for 24 and `admissions_suspended` for 1. The Ministry of Higher Education and Scientific Research (MoHESR) licensed list also shows 104 institutions, which matches the CAA active rows.

Use it to check whether a UAE college or university is licensed. Always confirm on the [CAA site](https://www.caa.ae/pages/institutes/all.aspx) before you rely on it for a study, hiring or visa decision.

> **Terms of use.** This API holds plain facts from a public government register (names, emirates, licence status, websites), with credit to the source. CAA and MoHESR state no reuse terms on these pages. CAA and MoHESR do not endorse this API. Map points from OpenStreetMap are under the ODbL, so credit OpenStreetMap contributors when you reuse them.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/uae_higher_education_institutions` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`emirate`). Optional: 9 fields. Disallowed: 5 fields.

`emirate` must be in every call. Use `emirate=*` to list all 160 institutions. Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `emirate` | Required | Yes | No | Emirate of the main licensed campus: `Dubai` (82), `Abu Dhabi` (43), `Sharjah` (17), `Ras Al Khaimah` (8), `Ajman` (6), `Fujairah` (3) or `Umm Al Quwain` (1). Wildcards work, so `emirate=*` lists all 160. |
| `caa_guid` | Optional | No | No | CAA record ID of the institution as text, such as `137`. One row per ID. |
| `institution_name` | Optional | Yes | No | Name in English, in upper case as CAA prints it, such as `*UNIVERSITY*` or `ABU DHABI*`. Matches ignore case, so `*university*` works too (91 rows). Former names and notes such as `(LICENSURE REVOKED)` are part of the name. |
| `institution_name_ar` | Optional | Yes | No | Name in Arabic as CAA publishes it, such as `*جامعة*` (81 rows). |
| `campus_emirates` | Optional | Yes | No | Every emirate CAA lists for the institution, comma separated, such as `*Sharjah*` (19 rows). Blank for 1 row. |
| `city` | Optional | Yes | No | City of the main campus, 11 values: `Dubai` (82), `Abu Dhabi` (35), `Sharjah` (13), `Al Ain` (8), `Ras Al Khaimah` (8), `Ajman` (6), `Fujairah` (3), `Khor Fakkan` (2), `Umm Al Quwain`, `Al Dhaid` and `Kalba` (1 each). |
| `licence_status` | Optional | No | No | CAA licence status: `active` (104), `inactive` (31), `licensure_revoked` (24) or `admissions_suspended` (1). |
| `latitude` | Optional | No | Yes | Latitude in decimal degrees (WGS 84) of the campus, area or city center, from 24.19194 to 25.788882. Example: `latitude=25..25.4`. |
| `longitude` | Optional | No | Yes | Longitude in decimal degrees (WGS 84), from 54.353464 to 56.353581. Example: `longitude=55.1..55.5`. |
| `geocode_quality` | Optional | No | No | How the point was placed: `osm_poi` (81, the campus in OpenStreetMap), `area` (36, the campus area, such as Dubai Knowledge Park), `city_centroid` (33, only the city center) or `wikidata` (10, the campus point in Wikidata). |
| `website` | Disallowed | No | No | Institution website as CAA lists it, changed to https. Blank for 27 rows. |
| `geocode_note` | Disallowed | No | No | Short note on the match used for the map point. |
| `caa_url` | Disallowed | No | No | Institution page on the CAA site. |
| `source` | Disallowed | No | No | Source of the row, CAA and MoHESR. |
| `source_as_of` | Disallowed | No | No | Date the data was read from CAA, `2026-10-29` on every row. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax. Arabic text in a URL must be percent encoded. With curl, use `-G --data-urlencode`.

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `caa_guid` | string | CAA record ID, as text. |
| `institution_name` | string | Name in English, upper case as CAA prints it. |
| `institution_name_ar` | string | Name in Arabic as CAA publishes it. |
| `emirate` | string | Emirate of the main licensed campus. |
| `campus_emirates` | string | Every emirate CAA lists, comma separated. Null for 1 row. |
| `city` | string | City of the main campus. |
| `licence_status` | string | `active`, `inactive`, `licensure_revoked` or `admissions_suspended`. |
| `website` | string | Institution website (https). Null for 27 rows. |
| `latitude` | number | Latitude in decimal degrees (WGS 84), 6 decimals. |
| `longitude` | number | Longitude in decimal degrees (WGS 84), 6 decimals. |
| `geocode_quality` | string | `osm_poi`, `area`, `city_centroid` or `wikidata`. |
| `geocode_note` | string | Short note on the match used for the map point. |
| `caa_url` | string | Institution page on the CAA site (https). |
| `source` | string | Source of the row. |
| `source_as_of` | string | Date the data was read from CAA. |

Notes:

* Map points are not always the campus. 33 rows have only a city center point (`city_centroid`) and 36 have an area point (`area`). Only `osm_poi` and `wikidata` rows point at the campus itself. CAA publishes no addresses or coordinates, so we added the points.
* Three institutions have campuses in more than one emirate: Higher Colleges of Technology (Dubai, Abu Dhabi, Sharjah, Fujairah, Ras Al Khaimah), Zayed University (Dubai, Abu Dhabi) and Khalifa University (Abu Dhabi, Sharjah). Each has one row and one point, in the first emirate CAA lists. `campus_emirates` lists all of them, so use `campus_emirates=*Sharjah*` to find every institution with a campus in Sharjah.
* CAA gives no emirate for Modul University Dubai, so its emirate is taken from its name and `campus_emirates` is blank.
* Rows follow the CAA list and are not merged. When a school has an old and a new licence, both rows show, such as BITS Pilani Dubai (`74` inactive and `1251` active).
* Websites are as CAA lists them, with http links changed to https. The links were not all tested. 27 rows have no website because CAA gives none or only a placeholder.
* Phone numbers and personal names are not included.

## Examples

Active institutions in Sharjah. This call returns 11 rows, from Al Qasimiya University to University of the Arts Sharjah:

```bash
curl -sG "https://api.jael.ee/JLEE/uae_higher_education_institutions" --data-urlencode "emirate=Sharjah" --data-urlencode "licence_status=active"
```

Response excerpt (the first row):

```json
[
  {
    "__id": 15,
    "caa_guid": "163",
    "institution_name": "AL QASIMIYA UNIVERSITY",
    "institution_name_ar": "جامعة القاسمية",
    "emirate": "Sharjah",
    "campus_emirates": "Sharjah",
    "city": "Sharjah",
    "licence_status": "active",
    "website": "https://www.alqasimia.ac.ae/ar/Pages/default.aspx",
    "latitude": 25.296667,
    "longitude": 55.465795,
    "geocode_quality": "osm_poi",
    "geocode_note": "Photon OSM POI",
    "caa_url": "https://www.caa.ae/Pages/Institutes/Details.aspx?GUID=163",
    "source": "UAE Commission for Academic Accreditation (CAA), Ministry of Higher Education and Scientific Research",
    "source_as_of": "2026-10-29"
  }
]
```

A wildcard on the name. Is the Sorbonne in Abu Dhabi licensed? This call returns 1 row, `active`. A wider search, `institution_name=*university*`, returns 91 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/uae_higher_education_institutions" --data-urlencode "emirate=*" --data-urlencode "institution_name=*sorbonne*" | jq -r '.[] | [.caa_guid, .institution_name, .licence_status] | @tsv'
```

```text
49	SORBONNE UNIVERSITY, ABU DHABI (FORMERLY: PARIS SORBONNE UNIVERSITY, ABU DHABI)	active
```

Institutions whose licence CAA has revoked. This call returns 24 rows, 14 of them in Dubai. The first three:

```bash
curl -sG "https://api.jael.ee/JLEE/uae_higher_education_institutions" --data-urlencode "emirate=*" --data-urlencode "licence_status=licensure_revoked" | jq -r '.[] | [.institution_name, .emirate] | @tsv'
```

```text
AL DAR UNIVERSITY COLLEGE (LICENSURE REVOKED)	Dubai
AL FALAH UNIVERSITY (LICENSURE REVOKED)	Dubai
AL GHURAIR UNIVERSITY (LICENSURE REVOKED)	Dubai
```

## Data source

The rows come from the UAE Commission for Academic Accreditation (CAA) [list of licensed institutions](https://www.caa.ae/pages/institutes/all.aspx) and its page for each institution, read on 2026-10-29. CAA is part of the Ministry of Higher Education and Scientific Research (MoHESR). The MoHESR licensed institutions page was used as a cross check, and its 104 institutions match the 104 CAA active rows. CAA and MoHESR state no reuse terms on these pages, so this API gives the register facts as plain facts with credit. Licence status, city and map points are set by us from the CAA data and other public sources, and the data may be out of date. Check the CAA site before you rely on it.

Map points: 150 of the 160 points come from OpenStreetMap (81 `osm_poi`, 36 `area` and 33 `city_centroid`), and 10 come from Wikidata (`wikidata`). No points were copied from commercial maps.

Credit lines:

* Source: UAE Commission for Academic Accreditation (CAA), Ministry of Higher Education and Scientific Research (https://www.caa.ae/pages/institutes/all.aspx). The Commission for Academic Accreditation and the Ministry of Higher Education and Scientific Research do not endorse or sponsor this API.
* Coordinates for 150 rows: © OpenStreetMap contributors, ODbL (https://www.openstreetmap.org/copyright).
* Coordinates for 10 rows: Wikidata, CC0.

[Back to the API list](../../README.md)
