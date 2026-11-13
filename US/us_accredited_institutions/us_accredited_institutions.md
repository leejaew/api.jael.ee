# US Accredited Institutions

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 6,463](https://img.shields.io/badge/records-6%2C463-lightgrey) ![updated 2026-11-13](https://img.shields.io/badge/updated-2026--11--13-orange) ![license public domain](https://img.shields.io/badge/license-public%20domain-green)

6,463 U.S. colleges and career schools with current institutional accreditation on the Department of Education DAPIP list, from files dated 13 November 2026. Each record has the school name, street address, city, state, the primary accreditor, status, first accredited date, next review date, and counts of current programs and extra locations. No field is required, so you can filter by state, accreditor, status, name, a review year or a location count range.

The largest states are CA (767), NY (448), FL (433), TX (396), PA (272), OH (267) and IL (236). 6,366 schools are Accredited and 97 are Preaccredited. The largest primary accreditors are the Higher Learning Commission (901) and the National Accrediting Commission of Career Arts and Sciences, Inc. (874).

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/us_accredited_institutions` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per school. All matches are returned. |

## Query parameters

Required: none. Optional: 16 fields. Disallowed: 1 field.

A call with no filters returns all 6,463 records (about 4 MB). Partial filters accept `*`, such as `name=*Phoenix*` or `accredited_since=2026*`. `programmatic_accreditation_count` and `location_count` accept a range with two dots, such as `location_count=10..999`. `state` and `accreditation_status` are exact match. Text filters ignore case. `source` cannot be filtered.

Disallowed fields are returned but cannot be used as filters. A filter on a disallowed field returns HTTP 400. Filters are combined with AND. See [How filters work](../../README.md#how-filters-work).

| Field | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `dapip_id` | Optional | Yes | No | Six digit DAPIP institution id, unique on every row, kept as text, such as 100025 or 101435. Partial wildcards allowed. |
| `name` | Optional | Yes | No | Institution name as DAPIP published it, such as Alabama A & M University or University of Phoenix. 6,249 distinct names; some names are shared by separate schools in different cities. Never blank. Partial wildcards allowed, such as `*Phoenix*` (10). |
| `address` | Optional | Yes | No | Street address, split from the DAPIP address line, such as 4900 Meridian Street. Blank on 3 rows where the source had no street (a leading comma): Niagara University, Oregon State University, and Covert & Associates Dale Carnegie Training. Partial wildcards allowed. |
| `city` | Optional | Yes | No | City, split from the DAPIP address line, such as Normal or University Park. Never blank. Partial wildcards allowed, such as `*Chicago*` (68). |
| `state` | Optional | No | No | Two letter state, territory or freely associated state code, in 59 values. The largest are CA (767), NY (448), FL (433), TX (396), PA (272), OH (267) and IL (236). Also PR (114), DC (37), GU (3), MP (2), VI (2), AS (1), FM (1), PW (1) and MH (1). Exact match. |
| `zip` | Optional | Yes | No | ZIP or ZIP+4 as text, so leading zeros remain, such as 01462 or 00717-1568. Blank on 24 rows where the source had no zip. One San Diego school is published as 00000. Partial wildcards allowed, such as `021*`. |
| `phone` | Optional | Yes | No | General institution phone, usually 10 digits, such as 2563725000. 364 numbers include an extension after a comma, such as 3077423776, ext. 0. Blank on 1,509 rows. Partial wildcards allowed. |
| `ope_id` | Optional | Yes | No | Eight digit Office of Postsecondary Education id, kept as text so leading zeros remain, such as 00100200. Blank on 1,530 rows (the source value -2 for St. Tikhon's Orthodox Theological Seminary was left blank). Partial wildcards allowed, such as `0010*`. |
| `ipeds_unit_id` | Optional | Yes | No | IPEDS unit id, kept as text, usually 6 digits, such as 100654. Blank on 1,337 rows. 13 schools have more than one id, joined with a comma and a space, such as 214777, 479956, 495767. Partial wildcards allowed. |
| `accreditor` | Optional | Yes | No | Primary institutional accrediting agency name, the agency with the earliest first accredited date (a tie is broken alphabetically). 40 agencies in all, counting second accreditors. The largest primary agencies are Higher Learning Commission (901), National Accrediting Commission of Career Arts and Sciences, Inc. (874), Southern Association of Colleges and Schools Commission on Colleges d.b.a. Commission on Colleges and Universities (767), Council on Occupational Education (594), Middle States Commission on Higher Education (490) and WASC Senior College and University Commission (197). Partial wildcards allowed, such as `*WASC*` (197). |
| `accreditation_status` | Optional | No | No | Status of the primary accreditor: Accredited (6,366) or Preaccredited (97). Exact match. |
| `accredited_since` | Optional | Yes | No | Date the primary accreditor first accredited the school, as text YYYY-MM-DD, from 1895-01-01 to 2026-10-20. 1,921 of these dates are marked Estimated in the source. Stored as text, so filter by an exact date or a prefix, such as `2026*` (99). Range is not supported on this field. |
| `next_review_date` | Optional | Yes | No | Next review date of the primary accreditor, as text YYYY-MM-DD, from 2008-12-31 to 2036-12-31, on 6,228 rows. Blank on 235 rows. 817 dates are before 13 November 2026; those schools are still current because the accreditation has no end date. Stored as text, so prefix filters such as `2027*` (891) work. Range is not supported. |
| `second_accreditor` | Optional | Yes | No | Other current institutional accreditor names, on 248 schools, earliest first. Blank on the rest. Four schools have two remaining agencies, joined with a semicolon and a space, such as Montessori Accreditation Council for Teacher Education; Accreditation Commission for Midwifery Education. Partial wildcards allowed, such as `*Commission*` (219). |
| `programmatic_accreditation_count` | Optional | No | Yes | Number of current programmatic accreditation records on the parent institution itself, from 0 to 37 (Ohio State University). 4,018 schools have 0 and 2,445 have at least one. Programs recorded only on an additional location or site are not added in. Range uses two dots, such as `programmatic_accreditation_count=0..0` (4,018) or `programmatic_accreditation_count=1..99` (2,445). A hyphen returns HTTP 400. |
| `location_count` | Optional | No | Yes | Number of Additional Location and Site rows under this parent, from 0 to 704 (University of Phoenix). 3,472 schools have 0 and 2,991 have at least one. Range uses two dots, such as `location_count=0..0` (3,472) or `location_count=10..999` (755). A hyphen returns HTTP 400. |
| `source` | Disallowed | No | No | U.S. Department of Education, DAPIP (ope.ed.gov/dapip), in all 6,463 rows. Not filterable. |

## Response fields

`__id`, `programmatic_accreditation_count` and `location_count` are integers. All other values are strings, or `null` when the source has no value.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. `dapip_id` is also unique on every row. |
| `dapip_id` | string | Six digit DAPIP institution id, kept as text. |
| `name` | string | Institution name as DAPIP published it. |
| `address` | string | Street address, or `null` on 3 rows. |
| `city` | string | City. |
| `state` | string | Two letter state, territory or freely associated state code. |
| `zip` | string | ZIP or ZIP+4 as text, or `null` on 24 rows. |
| `phone` | string | General institution phone, or `null` on 1,509 rows. |
| `ope_id` | string | Eight digit OPE id as text, or `null` on 1,530 rows. |
| `ipeds_unit_id` | string | IPEDS unit id as text, or `null` on 1,337 rows. |
| `accreditor` | string | Primary institutional accrediting agency. |
| `accreditation_status` | string | `Accredited` or `Preaccredited`. |
| `accredited_since` | string | First accredited date, YYYY-MM-DD. |
| `next_review_date` | string | Next review date, YYYY-MM-DD, or `null` on 235 rows. |
| `second_accreditor` | string | Other current institutional accreditor names, or `null` when there is only one. |
| `programmatic_accreditation_count` | integer | Current programmatic accreditation records on the parent institution, 0 to 37. |
| `location_count` | integer | Additional Location and Site rows under this parent, 0 to 704. |
| `source` | string | U.S. Department of Education, DAPIP (ope.ed.gov/dapip). |

## Data notes

* Each row is a parent institution with current institutional accreditation. Current means the status is Accredited or Preaccredited and the end date is empty or on or after 13 November 2026. Additional locations and sites are not separate rows; their counts are in `location_count`.
* The primary accreditor is the agency with the earliest first accredited date. A tie is broken alphabetically by agency name. Status, first accredited date and next review date come from that primary agency. 248 schools have a second institutional accreditor. Four schools have two remaining agencies, joined with a semicolon and a space.
* Ten schools are Accredited on the primary agency and Preaccredited on a later one, so a Preaccredited status filter returns 97 rows, not every school that has a Preaccredited record.
* 817 schools have a next review date before 13 November 2026. They are still treated as current because the accreditation has no end date.
* The source has no website field. ZIP codes and OPE ids keep their leading zeros.
* The accrediting agency's own directory is the final word on a school's status.

## Examples

Preaccredited schools in California. This call returns 15 records. The first is California College of Barbering and Cosmetology in Stockton, preaccredited since 2018-01-31:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/us_accredited_institutions' --data-urlencode 'state=CA' --data-urlencode 'accreditation_status=Preaccredited'
```

First record:

```json
[
  {
    "__id": 388,
    "dapip_id": "243735",
    "name": "California College of Barbering and Cosmetology",
    "address": "1024 East March Lane",
    "city": "Stockton",
    "state": "CA",
    "zip": "95210",
    "phone": null,
    "ope_id": "04277700",
    "ipeds_unit_id": "492661",
    "accreditor": "National Accrediting Commission of Career Arts and Sciences, Inc.",
    "accreditation_status": "Preaccredited",
    "accredited_since": "2018-01-31",
    "next_review_date": "2032-01-31",
    "second_accreditor": null,
    "programmatic_accreditation_count": 0,
    "location_count": 0,
    "source": "U.S. Department of Education, DAPIP (ope.ed.gov/dapip)"
  }
]
```

Schools whose name contains Phoenix. This call returns 10 records, including the University of Phoenix in Phoenix, Arizona, which has 704 additional locations:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/us_accredited_institutions' --data-urlencode 'name=*Phoenix*'
```

Schools with 10 or more additional locations. This call returns 755 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/us_accredited_institutions' --data-urlencode 'location_count=10..999'
```

## Data source

Source: [U.S. Department of Education, Database of Accredited Postsecondary Institutions and Programs (DAPIP)](https://ope.ed.gov/dapip/), files dated 13 November 2026. This is a United States government work and is in the public domain ([17 U.S.C. 105](https://www.govinfo.gov/content/pkg/USCODE-2023-title17/html/USCODE-2023-title17-chap1-sec105.htm)). The Department of Education [states that its information may be used without the Department's permission](https://www.ed.gov/about/ed-overview/required-notices/website-policies/copyright-status-notice-us-department-of-education). Personal administrator contacts were dropped. This use does not imply endorsement by the U.S. Department of Education. No Department seal or logos.
