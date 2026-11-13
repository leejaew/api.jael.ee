# US Cruise Ship Inspections

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 4,133](https://img.shields.io/badge/records-4%2C133-lightgrey) ![updated 2026-11-13](https://img.shields.io/badge/updated-2026--11--13-orange) ![license public domain](https://img.shields.io/badge/license-public%20domain-green)

4,133 cruise ship sanitation inspections by the CDC Vessel Sanitation Program, covering 261 ships from 54 cruise lines, from April 14, 1990 to October 17, 2026. CDC inspects ships that carry 13 or more passengers on an international trip and call at a US port. Each record has the ship, the cruise line, the inspection date, the score out of 100, a pass flag, a flag for the newest inspection of each ship, and a link to the CDC report. No field is required, so you can filter by ship, cruise line, date, score range, pass flag or only the latest inspection.

Most inspected lines: Carnival Cruise Lines, Inc. (721), Royal Caribbean International (697), Princess Cruises (344), Norwegian Cruise Lines (339), Holland America Line (285) and Celebrity Cruises (276). Scores run from 36 to 100. 672 inspections scored 100 and 77 scored 85 or lower, which CDC rates as not satisfactory. 223 inspections took place in 2026.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/us_cruise_ship_inspections` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per inspection. All matches are returned. |

## Query parameters

Required: none. Optional: 6 fields. Disallowed: 2 fields.

A call with no filters returns all 4,133 records (about 0.8 MB). Partial filters accept `*`, such as `cruise_line=Princess*` or `inspection_date=2026*`. `score` accepts a range with two dots, such as `score=0..85`. `satisfactory` and `is_latest` are exact match. Text filters ignore case. `report_url` and `source` cannot be filtered.

Disallowed fields are returned but cannot be used as filters. A filter on a disallowed field returns HTTP 400. Filters are combined with AND. See [How filters work](../../README.md#how-filters-work).

| Field | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `ship_name` | Optional | Yes | No | Ship name as CDC publishes it, such as Icon of the Seas or Emerald Princess. 261 ships, with 1 to 54 inspections each. Partial wildcards allowed, such as `*of the Seas` or `Carnival*`. |
| `cruise_line` | Optional | Yes | No | Cruise line or operator as CDC publishes it, 54 values, such as Royal Caribbean International, Princess Cruises or Carnival Cruise Lines, Inc. Spellings are kept as published, including Viking Epedition Operations. Each ship has one line. Partial wildcards allowed, such as `Princess*` or `*Viking*`. |
| `inspection_date` | Optional | Yes | No | Inspection date as text, YYYY-MM-DD, from 1990-04-14 to 2026-10-17. Filter by an exact date or a prefix, such as `2026*` for a year or `2025-07*` for a month. Range does not work on this field. There are no inspections in 2021. |
| `score` | Optional | No | Yes | Inspection score out of 100 as published, from 36 to 100. Range uses two dots, such as `0..85` for failed inspections or `100..100` for perfect scores. A hyphen returns HTTP 400. |
| `satisfactory` | Optional | No | No | `Yes` if the score is 86 or higher, `No` if it is 85 or lower, following the CDC rule. Yes (4,056); No (77). Exact match. |
| `is_latest` | Optional | No | No | `Yes` on the newest inspection of each ship (261 records), `No` on older ones (3,872). Exact match. See the data notes for how this differs from the CDC Green Sheet. |
| `report_url` | Disallowed | No | No | Link to the CDC inspection report, one per record. Not filterable. |
| `source` | Disallowed | No | No | CDC Vessel Sanitation Program, in every record. Not filterable. |

## Response fields

`__id` and `score` are integers. All other values are strings. No field is blank.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. CDC publishes no plain inspection ID. |
| `ship_name` | string | Ship name as CDC publishes it. |
| `cruise_line` | string | Cruise line or operator as CDC publishes it. |
| `inspection_date` | string | Inspection date, YYYY-MM-DD. |
| `score` | integer | Inspection score out of 100. |
| `satisfactory` | string | `Yes` for 86 or higher, `No` for 85 or lower. |
| `is_latest` | string | `Yes` on the newest inspection of each ship. |
| `report_url` | string | Link to the CDC inspection report. |
| `source` | string | CDC Vessel Sanitation Program. |

## Data notes

* `is_latest` marks the newest inspection of every ship in the data, 261 ships in all. The CDC Green Sheet lists the latest score for 205 ships, and every one of them matches our `is_latest` record on date, score and report link. The other 56 ships are not on the Green Sheet, mostly because CDC no longer inspects them, so `is_latest=Yes` returns a few more ships than the Green Sheet: 165 inspected in 2026 instead of 164, 43 with a score of 100 instead of 41, and 6 at 85 or lower instead of 4. Add `inspection_date=2026*` to see only ships inspected this year.
* The CDC report site may send a report link to its search page the first time. Open the [CDC inspection search](https://wwwn.cdc.gov/InspectionQueryTool/InspectionSearch.aspx) once, and the report links then open in the same browser.
* Dates are shown by CDC as M/D/YYYY and are given here as YYYY-MM-DD. Scores, ship names and cruise line names are kept as published.
* CDC lists some ships and cruise lines in its search form that have no inspections. They are not in this data.
* Inspections from 1990 to 2000 are fewer (164 in all) than in later years. CDC carried out few inspections in 2020 and 2022 and none in 2021.
* The data has no personal information.

## Examples

Failed inspections in 2026. This call returns 3 records: Emerald Princess (78), Le Lyrial (83) and Norwegian Dawn (84).

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/us_cruise_ship_inspections' --data-urlencode 'score=0..85' --data-urlencode 'inspection_date=2026*'
```

First record:

```json
[
  {
    "__id": 1721,
    "ship_name": "Emerald Princess",
    "cruise_line": "Princess Cruises",
    "inspection_date": "2026-10-16",
    "score": 78,
    "satisfactory": "No",
    "is_latest": "Yes",
    "report_url": "https://wwwn.cdc.gov/InspectionQueryTool/InspectionDetailReport.aspx?ColI=MjM2ODA2Mzk%3d-4TG5ITfNwx8%3d",
    "source": "CDC Vessel Sanitation Program"
  }
]
```

Latest inspection of each Princess Cruises ship. This call returns 17 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/us_cruise_ship_inspections' --data-urlencode 'cruise_line=Princess*' --data-urlencode 'is_latest=Yes'
```

Every inspection of Icon of the Seas. This call returns 5 records; the newest, on 2026-09-05, scored 100:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/us_cruise_ship_inspections' --data-urlencode 'ship_name=Icon of the Seas'
```

## Data source

Source: CDC Vessel Sanitation Program. Records come from the [CDC cruise ship inspection search](https://wwwn.cdc.gov/InspectionQueryTool/InspectionSearch.aspx) (all ships, all dates) and were checked against the [CDC Green Sheet](https://wwwn.cdc.gov/inspectionquerytool/InspectionGreenSheetRpt.aspx). Data pulled November 13, 2026, Asia/Singapore. This data is available free on the CDC website. CDC states that most of its information [is in the public domain, and may be freely used or reproduced without obtaining copyright permission](https://www.cdc.gov/other/agencymaterials.html), and as a work of the US government it is not subject to copyright in the US ([17 U.S.C. § 105](https://www.govinfo.gov/content/pkg/USCODE-2023-title17/html/USCODE-2023-title17-chap1-sec105.htm)).

Credit: CDC Vessel Sanitation Program. Use of this data, including any links to the CDC website, does not imply endorsement by CDC, HHS or the United States Government of this API or its publisher. No CDC or HHS logo or mark is used. Check the CDC site for the newest scores, as CDC adds inspections often.
