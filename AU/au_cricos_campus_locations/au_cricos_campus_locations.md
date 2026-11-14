# Australian CRICOS Campus Locations

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 3,855](https://img.shields.io/badge/records-3%2C855-lightgrey) ![updated 2026-11-02](https://img.shields.io/badge/updated-2026--11--02-orange) ![license CC BY 2.5 AU](https://img.shields.io/badge/license-CC%20BY%202.5%20AU-green)

3,855 campus locations of 1,539 Australian education providers on the Commonwealth Register of Institutions and Courses for Overseas Students (CRICOS) as of 2 November 2026. Each record has the provider code, trading and institution names, government or private type, overseas student cap, website, campus name, address, city, state and postcode. No field is required, so you can filter by state, city, postcode, provider, institution name, campus type or a capacity range.

The largest states are VIC (1,139), NSW (1,052), QLD (680), WA (406) and SA (340). 2,762 campuses belong to private providers and 1,093 to government providers.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/au_cricos_campus_locations` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per campus. All matches are returned. |

## Query parameters

Required: none. Optional: 15 fields. Disallowed: 2 fields.

A call with no filters returns all 3,855 records. Partial filters accept `*`, such as `institution_name=*University of Sydney*`, `city=*Haymarket*` or `postcode=0*`. `institution_capacity` accepts a range with two dots, such as `institution_capacity=10000..44600`. `institution_type`, `location_type` and `state` are exact match. Text filters ignore case. `source` and `source_as_of` cannot be filtered.

Disallowed fields are returned but cannot be used as filters. A filter on a disallowed field returns HTTP 400. Filters are combined with AND. See [How filters work](../../README.md#how-filters-work).

| Field | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `cricos_provider_code` | Optional | Yes | No | Official six character CRICOS provider code, always starting with 0 and ending with a letter, such as 00001K (Canberra Institute of Technology, 3 campuses) or 00025B (The University of Queensland). Text, so the leading zero is kept. Never blank. Partial wildcards allowed, such as `00001*` or `*01K`. |
| `trading_name` | Optional | Yes | No | Trading name from the institution record, such as Lexis English Perth or Sydney Catholic Schools. Blank on 630 rows where CRICOS lists none. Partial wildcards allowed, such as `*Lexis English*` (6). |
| `institution_name` | Optional | Yes | No | Legal institution name as CRICOS lists it, such as Canberra Institute of Technology or The University of Queensland. Never blank. 1,506 distinct names across 1,539 providers. Partial wildcards allowed, such as `*University of Sydney*` (10) or `*TAFE*` (43). |
| `institution_type` | Optional | No | No | Government (1093 campus rows, 65 providers, such as TAFE institutes and public universities) or Private (2762 campus rows, 1,474 providers). This is the provider type, not a campus type. Exact match. |
| `institution_capacity` | Optional | No | Yes | Maximum number of overseas students the provider may enroll, from 4 to 44600. This cap belongs to the provider, not to one campus, so every campus of that provider carries the same figure. Never blank. Use a range, for example `institution_capacity=10000..44600` (179) or `institution_capacity=4..50` (325). A hyphen returns HTTP 400. |
| `website` | Optional | Yes | No | Provider website as CRICOS lists it, such as https://international.cit.edu.au/ or www.uq.edu.au. Some values have no http or https prefix (1797 rows). Blank on 1115 rows. Two campus rows for Phoenix College of Australia Pty Ltd (03871F) list the email info@newgen.edu.au in this field, as published. Partial wildcards allowed, such as `*edu.au*` (2126). |
| `location_name` | Optional | Yes | No | Campus or site name as CRICOS lists it, such as Herston Campus or CIT Woden. Never blank. Four names are used at more than one address, including Haileybury College (3 campuses). Some names also repeat the street, suburb and postcode. Partial wildcards allowed, such as `*Haymarket*` (47). |
| `location_type` | Optional | No | No | How the campus is run, in three CRICOS values: Location owned and operated by provider (3609), Arrangement with Non Registered Provider (168) or Arrangement with other registered provider (78). Exact match, using that full wording. |
| `address_line_1` | Optional | Yes | No | First line of the campus street address as CRICOS lists it, such as Vowels cres or 149 Victoria Park Rd. Never blank. Casing follows the source, so some lines are in capitals. Partial wildcards allowed, such as `Level*` (615) or `*Level*` (756). |
| `address_line_2` | Optional | Yes | No | Second address line when CRICOS gives one, such as a building, level or extra street. Blank on 3332 rows. Partial wildcards allowed, such as `*Level*` or `*Campus*`. |
| `address_line_3` | Optional | Yes | No | Third address line when CRICOS gives one, such as a building or extra street. Blank on 3814 rows. Partial wildcards allowed. |
| `address_line_4` | Optional | Yes | No | Fourth address line when CRICOS gives one, such as a unit, building or lot. Blank on 3827 rows. Partial wildcards allowed. |
| `city` | Optional | Yes | No | Suburb or locality of the campus, in title case, such as Melbourne (370), Sydney (189), Adelaide (150), Parramatta (103) or Haymarket (70). Never blank. Matching is case insensitive, so `city=melbourne` returns the same 370 rows. Partial wildcards allowed, such as `*Haymarket*` (70) or `North*`. |
| `state` | Optional | No | No | Australian state or territory abbreviation in eight values: VIC (1139), NSW (1052), QLD (680), WA (406), SA (340), ACT (148), TAS (63) and NT (27). Exact match. |
| `postcode` | Optional | Yes | No | Four digit Australian postcode as text, so leading zeros remain, such as 0800 (Darwin City) or 2617 (Bruce). Never blank. Partial wildcards allowed, such as `2000` (265, mostly Sydney CBD, plus a few rows whose postcode does not fit the usual state range) or `0*` (27, all in the Northern Territory). |
| `source` | Disallowed | No | No | Department of Education, Commonwealth Register of Institutions and Courses for Overseas Students (CRICOS), in all 3855 rows. Not filterable. |
| `source_as_of` | Disallowed | No | No | Date of the CRICOS locations and institutions snapshot, YYYY-MM-DD (`2026-11-02`). Text. Not filterable. |

## Response fields

`__id` and `institution_capacity` are integers. All other values are strings, or `null` when the source has no value.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. CRICOS gives no campus id, and a provider code plus location name is not unique, so use this as the key. |
| `cricos_provider_code` | string | Official six character CRICOS provider code, with the leading zero kept. |
| `trading_name` | string | Trading name, or `null` on 630 rows. |
| `institution_name` | string | Legal institution name as CRICOS lists it. |
| `institution_type` | string | `Government` or `Private`. This is the provider type. |
| `institution_capacity` | integer | Maximum number of overseas students the provider may enroll. The same figure appears on every campus of that provider. |
| `website` | string | Provider website as CRICOS lists it, or `null` on 1,115 rows. |
| `location_name` | string | Campus or site name as CRICOS lists it. |
| `location_type` | string | How the campus is run. |
| `address_line_1` | string | First line of the campus street address. |
| `address_line_2` | string | Second address line, or `null` on 3,332 rows. |
| `address_line_3` | string | Third address line, or `null` on 3,814 rows. |
| `address_line_4` | string | Fourth address line, or `null` on 3,827 rows. |
| `city` | string | Suburb or locality, in title case. |
| `state` | string | Australian state or territory abbreviation. |
| `postcode` | string | Four digit postcode as text, with leading zeros kept. |
| `source` | string | Department of Education, Commonwealth Register of Institutions and Courses for Overseas Students (CRICOS). |
| `source_as_of` | string | Date of the snapshot, `2026-11-02`. |

## Data notes

* `institution_capacity` is the provider cap, not a campus cap. Every campus of that provider carries the same figure.
* City names are in title case. A few values that also repeated the state or postcode were trimmed, and three city values were filled from other fields on the same official row.
* Seven rows have a postcode that does not match the usual range for that state. They are kept as published. `city=Sydney` returns 189 rows, of which 188 are in NSW and one is listed as QLD 2000.
* Provider codes and Northern Territory postcodes keep their leading zeros, such as 00001K and 0800.
* Some websites have no http or https prefix. Two campus rows list the email info@newgen.edu.au in `website`, as CRICOS published them.
* There are no map points. The CRICOS locations file has no coordinates.
* This is a snapshot from 2 November 2026. The live register is on the CRICOS website.

## Examples

Campuses of The University of Sydney. This call returns 10 records. The first two:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/au_cricos_campus_locations' --data-urlencode 'institution_name=*University of Sydney*'
```

```json
[
  {
    "__id": 201,
    "cricos_provider_code": "00026A",
    "trading_name": "The University of Sydney",
    "institution_name": "The University of Sydney",
    "institution_type": "Government",
    "institution_capacity": 44600,
    "website": "http://sydney.edu.au",
    "location_name": "Camden Campus (Plant Breeding Institute)",
    "location_type": "Location owned and operated by provider",
    "address_line_1": "107 Cobbitty Rd",
    "address_line_2": null,
    "address_line_3": null,
    "address_line_4": null,
    "city": "Cobbity",
    "state": "NSW",
    "postcode": "2570",
    "source": "Department of Education, Commonwealth Register of Institutions and Courses for Overseas Students (CRICOS)",
    "source_as_of": "2026-11-02"
  },
  {
    "__id": 202,
    "cricos_provider_code": "00026A",
    "trading_name": "The University of Sydney",
    "institution_name": "The University of Sydney",
    "institution_type": "Government",
    "institution_capacity": 44600,
    "website": "http://sydney.edu.au",
    "location_name": "Camden Campus (Veterinary Science)",
    "location_type": "Location owned and operated by provider",
    "address_line_1": "425 Werombi Rd",
    "address_line_2": null,
    "address_line_3": null,
    "address_line_4": null,
    "city": "Camden",
    "state": "NSW",
    "postcode": "2570",
    "source": "Department of Education, Commonwealth Register of Institutions and Courses for Overseas Students (CRICOS)",
    "source_as_of": "2026-11-02"
  }
]
```

Every campus in Sydney, New South Wales. This call returns 188 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/au_cricos_campus_locations' --data-urlencode 'state=NSW' --data-urlencode 'city=Sydney'
```

Providers allowed to enroll 10,000 or more overseas students. This call returns 179 campus rows:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/au_cricos_campus_locations' --data-urlencode 'institution_capacity=10000..44600'
```

## Data source

Source: [Department of Education, Commonwealth Register of Institutions and Courses for Overseas Students (CRICOS)](https://cricos.education.gov.au/), from the [CRICOS dataset on data.gov.au](https://data.gov.au/data/dataset/cricos), snapshot of 2 November 2026. Licensed under the [Creative Commons Attribution 2.5 Australia (CC BY 2.5 AU)](https://creativecommons.org/licenses/by/2.5/au/) license, which lets anyone copy, change and use the data, including for commercial use, as long as the source is credited and changes are noted.

This API joins the official locations and institutions files, converts city names to title case, and stores the provider's overseas student cap as an integer. Seven rows have a postcode that does not match the usual range for that state; those values are kept as published. There are no map points, because the CRICOS locations file has no coordinates. For the latest data visit [cricos.education.gov.au](https://cricos.education.gov.au/).

Use of this data does not mean the Department of Education endorses this API or its maker.

[Back to the API list](../../README.md)
