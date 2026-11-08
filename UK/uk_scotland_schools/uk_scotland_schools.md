# Scotland Schools

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 2,426](https://img.shields.io/badge/records-2%2C426-lightgrey) ![updated 2026-09-28](https://img.shields.io/badge/updated-2026--09--28-orange) ![license OGL v3](https://img.shields.io/badge/license-OGL%20v3-green)

Every publicly funded school open in Scotland on 31 July 2026: 2,419 council schools in 32 councils plus 7 grant aided schools. Each record has the seed code, address, postcode, UPRN, phone, website, departments, Gaelic teaching, denomination and a map point. Choose a council, then narrow by sector, Gaelic medium, denomination, name, postcode or a lat and lng box.

Schools by department: primary (1,970), pre school (1,394), secondary (362) and special (122). 95 schools teach in Gaelic.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/uk_scotland_schools` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per school. All matches are returned. |

## Query parameters

Required: 1 field (`la_name`). Optional: 26 fields. Disallowed: 2 fields.

`la_name` must be in every call and must match an exact council name. It does not accept wildcards. Partial filters accept `*`, such as `school_name=*Academy*`. `lat` and `lng` accept ranges with two dots, such as `lat=55.9..56.0`. All other fields are exact match strings. The text date `updated` cannot be filtered.

Disallowed fields are returned but cannot be used as filters. A call without `la_name`, or a filter on a disallowed field, returns HTTP 400. Filters are combined with AND. See [How filters work](../../README.md#how-filters-work).

| Field | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `la_name` | Required | No | No | Required exact council name as published. Grant aided schools use Grant Aided. Values: Aberdeen City (61); Aberdeenshire (161); Angus (59); Argyll & Bute (81); Clackmannanshire (24); Dumfries & Galloway (108); Dundee City (42); East Ayrshire (54); East Dunbartonshire (44); East Lothian (41); East Renfrewshire (32); Edinburgh City (124); Falkirk (58); Fife (158); Glasgow City (192); Grant Aided (7); Highland (194); Inverclyde (28); Midlothian (39); Moray (52); Na h-Eileanan Siar (23); North Ayrshire (59); North Lanarkshire (154); Orkney Islands (21); Perth & Kinross (77); Renfrewshire (63); Scottish Borders (65); Shetland Islands (27); South Ayrshire (51); South Lanarkshire (150); Stirling (49); West Dunbartonshire (41); West Lothian (87). |
| `seed_code` | Optional | No | No | Scottish Government seed code, the school ID. Unique per row. Exact match, such as 1000047. |
| `school_name` | Optional | Yes | No | School name as published. Partial wildcards allowed, such as *Academy* or St*. |
| `centre_type` | Optional | No | No | Local Authority (2419) or Grant Aided (7). Exact match. |
| `address_line1` | Optional | Yes | No | First address line, often the street. Partial wildcards allowed. |
| `address_line2` | Optional | Yes | No | Second address line, often the area or town. Blank in 407 rows. Partial wildcards allowed. |
| `address_line3` | Optional | Yes | No | Third address line, often the post town. All caps source values changed to title case. Blank in 1040 rows. Partial wildcards allowed. |
| `postcode` | Optional | Yes | No | Postcode with its usual space, such as G12 9PQ. Partial wildcards allowed, such as G12*. |
| `uprn` | Optional | No | No | Unique Property Reference Number, the official property ID, as text. Blank in 2 rows where the source gave 0. Some sites share one UPRN. Exact match. |
| `phone` | Optional | No | No | Main school phone in UK format. Extensions and second numbers dropped. Blank in 8 rows with no usable number. Exact match. |
| `website` | Optional | Yes | No | School website. Changed to https only where the https address answered; others kept as published. Some links may be out of date. Blank in 9 rows. |
| `pre_school` | Optional | No | No | Has a pre school department: Yes or No (Yes 1394, No 1032). Exact match. |
| `primary` | Optional | No | No | Has a primary department: Yes or No (Yes 1970, No 456). Exact match. |
| `secondary` | Optional | No | No | Has a secondary department: Yes or No (No 2064, Yes 362). Exact match. |
| `special` | Optional | No | No | Has a special department: Yes or No (No 2304, Yes 122). Exact match. |
| `sectors` | Optional | Yes | No | Departments joined with commas, such as Pre school, Primary. Partial wildcards allowed, such as *Secondary*. |
| `integrated_special_unit` | Optional | No | No | Has an integrated special unit: Yes or No (No 2025, Yes 401). Exact match. |
| `gaelic_learners` | Optional | No | No | Offers Gaelic for learners: Yes, No, or blank when not stated (No 2042, blank 236, Yes 148). Exact match. |
| `medium_of_education` | Optional | Yes | No | Teaching language as published: English medium only (2004); English medium (but has Gaelic Medium stream) (59); Gaelic medium (but has English Medium stream) (23); Gaelic medium only (13); blank (327). Partial wildcards allowed, such as Gaelic*. |
| `gaelic_medium` | Optional | No | No | Yes when the school teaches in Gaelic, alone or as a stream (95). No when English medium only (2004). Blank when not stated (327). Exact match. |
| `denomination` | Optional | No | No | Denomination as published: Non-denominational (2057); Roman Catholic (357); Inter-denominational (7); Episcopalian (3); Multi-denominational (1); Jewish (1). Exact match. |
| `half_day_openings_per_week` | Optional | No | No | Half day sessions per week as text: 10 (1908), 9 (505), blank (13). Exact match. |
| `primary_pe_target` | Optional | No | No | Meets the primary PE target of two hours a week: Yes, No, or blank (Yes 1953, blank 459, No 14). Exact match. |
| `secondary_pe_target` | Optional | No | No | Meets the secondary PE target of two periods a week: Yes, No, or blank (blank 2067, Yes 341, No 18). Exact match. |
| `lat` | Optional | No | Yes | Latitude in decimal degrees. Range filter with two dots, such as 55.9..56.0. |
| `lng` | Optional | No | Yes | Longitude in decimal degrees, negative for west. Range filter with two dots, such as -3.3..-3.1. |
| `coord_source` | Optional | No | No | How lat and lng were found: OS Open UPRN (2421) or Postcode centroid (5). Exact match. |
| `source` | Disallowed | No | No | Source page URL. Not filterable. |
| `updated` | Disallowed | No | No | Date the source file was last updated, 2026-09-28, as text. Not filterable. |

## Response fields

Blank source cells are returned as `null`. `lat` and `lng` are numbers. All other values are strings.

| Field | Type | Description |
|---|---|---|
| `seed_code` | string | Scottish Government seed code, the school ID. Unique per row. |
| `school_name` | string | School name as published. |
| `la_name` | string | Required exact council name as published. Grant aided schools use Grant Aided. Values: Aberdeen City (61); Aberdeenshire (161); Angus (59); Argyll & Bute (81); Clackmannanshire (24); Dumfries & Galloway (108); Dundee City (42); East Ayrshire (54); East Dunbartonshire (44); East Lothian (41); East Renfrewshire (32); Edinburgh City (124); Falkirk (58); Fife (158); Glasgow City (192); Grant Aided (7); Highland (194); Inverclyde (28); Midlothian (39); Moray (52); Na h-Eileanan Siar (23); North Ayrshire (59); North Lanarkshire (154); Orkney Islands (21); Perth & Kinross (77); Renfrewshire (63); Scottish Borders (65); Shetland Islands (27); South Ayrshire (51); South Lanarkshire (150); Stirling (49); West Dunbartonshire (41); West Lothian (87). |
| `centre_type` | string | Local Authority (2419) or Grant Aided (7). |
| `address_line1` | string | First address line, often the street. |
| `address_line2` | string | Second address line, often the area or town. Blank in 407 rows. |
| `address_line3` | string | Third address line, often the post town. All caps source values changed to title case. Blank in 1040 rows. |
| `postcode` | string | Postcode with its usual space, such as G12 9PQ. |
| `uprn` | string | Unique Property Reference Number, the official property ID, as text. Blank in 2 rows where the source gave 0. Some sites share one UPRN. |
| `phone` | string | Main school phone in UK format. Extensions and second numbers dropped. Blank in 8 rows with no usable number. |
| `website` | string | School website. Changed to https only where the https address answered; others kept as published. Some links may be out of date. Blank in 9 rows. |
| `pre_school` | string | Has a pre school department: Yes or No (Yes 1394, No 1032). |
| `primary` | string | Has a primary department: Yes or No (Yes 1970, No 456). |
| `secondary` | string | Has a secondary department: Yes or No (No 2064, Yes 362). |
| `special` | string | Has a special department: Yes or No (No 2304, Yes 122). |
| `sectors` | string | Departments joined with commas, such as Pre school, Primary. |
| `integrated_special_unit` | string | Has an integrated special unit: Yes or No (No 2025, Yes 401). |
| `gaelic_learners` | string | Offers Gaelic for learners: Yes, No, or blank when not stated (No 2042, blank 236, Yes 148). |
| `medium_of_education` | string | Teaching language as published: English medium only (2004); English medium (but has Gaelic Medium stream) (59); Gaelic medium (but has English Medium stream) (23); Gaelic medium only (13); blank (327). |
| `gaelic_medium` | string | Yes when the school teaches in Gaelic, alone or as a stream (95). No when English medium only (2004). Blank when not stated (327). |
| `denomination` | string | Denomination as published: Non-denominational (2057); Roman Catholic (357); Inter-denominational (7); Episcopalian (3); Multi-denominational (1); Jewish (1). |
| `half_day_openings_per_week` | string | Half day sessions per week as text: 10 (1908), 9 (505), blank (13). |
| `primary_pe_target` | string | Meets the primary PE target of two hours a week: Yes, No, or blank (Yes 1953, blank 459, No 14). |
| `secondary_pe_target` | string | Meets the secondary PE target of two periods a week: Yes, No, or blank (blank 2067, Yes 341, No 18). |
| `lat` | number | Latitude in decimal degrees. |
| `lng` | number | Longitude in decimal degrees, negative for west. |
| `coord_source` | string | How lat and lng were found: OS Open UPRN (2421) or Postcode centroid (5). |
| `source` | string | Source page URL. |
| `updated` | string | Date the source file was last updated, 2026-09-28, as text. |

## Data notes

* The source Email column was removed, since many addresses name staff. The source has no head teacher or other person name columns.
* Phone extensions, internal numbers and second numbers were dropped. Eight phones with no usable number are blank.
* Two rows where the source gave UPRN 0 are blank. Some campuses share one UPRN.
* OS Open UPRN gave map points for 2,421 of 2,426 schools (99.8%). Five schools use a postcode centroid, shown in `coord_source`. Twelve UPRN points sit more than 2 km from their postcode centroid; these may be source UPRN or postcode errors.
* Website links were changed to https only where the https address answered. Some links may be out of date.
* Changes from the source: field names, whitespace cleanup, title case for all caps address lines, phone format, https where it worked, added `sectors`, `gaelic_medium`, `lat`, `lng` and `coord_source`, email removed.

## Examples

Gaelic medium schools in Highland with Inverness in the name. This call returns 1 record:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_scotland_schools' --data-urlencode la_name=Highland --data-urlencode gaelic_medium=Yes --data-urlencode 'school_name=*Inverness*'
```

Fields trimmed:

```json
[
  {
    "seed_code": "5142938",
    "school_name": "Inverness Royal Academy",
    "la_name": "Highland",
    "postcode": "IV2 6RE",
    "secondary": "Yes",
    "medium_of_education": "English medium (but has Gaelic Medium stream)",
    "gaelic_medium": "Yes",
    "lat": 57.449757,
    "lng": -4.225598,
    "updated": "2026-09-28"
  }
]
```

Schools in a central Edinburgh box. This call returns 103 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_scotland_schools' --data-urlencode 'la_name=Edinburgh City' --data-urlencode lat=55.9..56.0 --data-urlencode lng=-3.3..-3.1
```

## Data source

[Scottish Government, School contact details](https://www.gov.scot/publications/school-contact-details/), school contact list as of 31 July 2026, updated 28 September 2026.

Contains public sector information licensed under the [Open Government Licence v3.0 (OGL v3)](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/). You may copy, share, adapt and use it for business if you credit the source. Logos are not covered. The license gives no warranty or endorsement.

Map points: [OS Open UPRN](https://osdatahub.os.uk/downloads/open/OpenUPRN), October 2026 release. Contains OS data (c) Crown copyright and database right 2026, Open Government Licence v3.0. Five schools use a postcode centroid from postcodes.io (ONS and OS data, Open Government Licence v3.0).

The source asks that business callers respect any Telephone Preference Service listing, and ask the local council for permission before they contact schools directly.
