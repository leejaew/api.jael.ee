# Northern Ireland Schools

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 1,116](https://img.shields.io/badge/records-1%2C116-lightgrey) ![updated 2026-11-10](https://img.shields.io/badge/updated-2026--11--10-orange) ![license OGL v3](https://img.shields.io/badge/license-OGL%20v3-green)

Every open school and further education college in the Department of Education (Northern Ireland) institution list on 10 November 2026: 1,116 in all six counties. Each record has the DE reference number, school type, management type, address, postcode, phone, school email and, for 1,044 sites, a map point. Choose a county, then narrow by type, management, town, name, postcode or a lat and lng box.

Schools by type: primary (760), secondary (122), nursery (93), grammar (66), special (40), independent (15), preparatory departments (10) and further education colleges (10).

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/uk_ni_schools` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per school. All matches are returned. |

## Query parameters

Required: 1 field (`county`). Optional: 14 fields. Disallowed: 2 fields.

`county` must be in every call and must match one of the six county names exactly. It does not accept wildcards. Partial filters accept `*`, such as `name=Gaelscoil*` or `town=*Belfast*`. `lat` and `lng` accept ranges with two dots, such as `lat=54.55..54.65`. All other fields are exact match strings. The text date `updated` cannot be filtered.

Disallowed fields are returned but cannot be used as filters. A call without `county`, or a filter on a disallowed field, returns HTTP 400. Filters are combined with AND. See [How filters work](../../README.md#how-filters-work).

| Field | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `county` | Required | No | No | Required exact county name. Values: Antrim (411); Down (240); Tyrone (157); Londonderry (155); Armagh (100); Fermanagh (53). |
| `de_ref` | Optional | No | No | Department of Education reference number as text. 1,091 schools use the form 123-4567, such as 101-0012. The 15 independent schools use IS numbers, such as IS30. Blank for the 10 further education colleges, which have no number in the source. Unique where present. Exact match. |
| `name` | Optional | Yes | No | School or college name as published. Partial wildcards allowed, such as Gaelscoil* or *Grammar*. |
| `school_type` | Optional | No | No | Institution type as published. Values: Primary (760); Secondary (122); Nursery (93); Grammar (66); Special (40); Independent (15); Preps (10, preparatory departments); Further Education (10). Exact match. |
| `management_type` | Optional | No | No | Management type as published. Values: Controlled (504); Roman Catholic Maintained (429); Voluntary (59); Controlled Integrated (39); Grant Maintained Integrated (37); Other Maintained (29); Other (19). Exact match. |
| `address_line1` | Optional | Yes | No | First address line, often the street. All caps source values changed to title case. Partial wildcards allowed, such as *Road*. |
| `address_line2` | Optional | Yes | No | Second address line, often the area or village. Blank in 663 rows. Partial wildcards allowed. |
| `address_line3` | Optional | Yes | No | Third address line. Blank in 1,099 rows. Partial wildcards allowed. |
| `town` | Optional | Yes | No | Post town in title case. 71 towns, none blank. Belfast has 188 rows (185 in Antrim, 3 in Down). Partial wildcards allowed, such as *Belfast* or Newtown*. |
| `postcode` | Optional | Yes | No | Postcode with one space, such as BT4 2AG. Partial wildcards allowed, such as BT9*. |
| `phone` | Optional | No | No | Main phone as text in UK format, such as 028 9047 1539. Only the first number is kept where two were given. Blank in 1 row. Exact match. |
| `email` | Optional | No | No | School contact email. 1,087 use the school info address on ni.sch.uk and 11 use a school or college address on its own domain. Blank in 18 rows. Exact match. |
| `lat` | Optional | No | Yes | Latitude in decimal degrees from the 2016 DE school locations file. Blank in 72 rows. Range filter with two dots, such as 54.55..54.65. |
| `lng` | Optional | No | Yes | Longitude in decimal degrees, negative for west. Blank in 72 rows. Range filter with two dots, such as -6..-5.85. |
| `coord_source` | Optional | No | No | Where the map point came from: DE 2016 school locations (1,044), or blank when there is no point (72). Exact match. |
| `source` | Disallowed | No | No | Source name and license. Not filterable. |
| `updated` | Disallowed | No | No | Date the source list was read, 2026-11-10, as text. Not filterable. |

## Response fields

Blank source cells are returned as `null`. `lat` and `lng` are numbers. All other values are strings.

| Field | Type | Description |
|---|---|---|
| `de_ref` | string | Department of Education reference number. Form 123-4567 for 1,091 schools, IS numbers for 15 independent schools, null for the 10 colleges. |
| `name` | string | School or college name as published. |
| `school_type` | string | Primary, Secondary, Nursery, Grammar, Special, Independent, Preps or Further Education. |
| `management_type` | string | Controlled, Roman Catholic Maintained, Voluntary, Controlled Integrated, Grant Maintained Integrated, Other Maintained or Other. |
| `address_line1` | string | First address line, often the street. |
| `address_line2` | string | Second address line, often the area or village. Null in 663 rows. |
| `address_line3` | string | Third address line. Null in 1,099 rows. |
| `town` | string | Post town. |
| `county` | string | Antrim, Down, Tyrone, Londonderry, Armagh or Fermanagh. |
| `postcode` | string | Postcode with one space. |
| `phone` | string | Main phone in UK format. Null in 1 row. |
| `email` | string | School contact email. Null in 18 rows. |
| `lat` | number | Latitude in decimal degrees. Null in 72 rows. |
| `lng` | number | Longitude in decimal degrees, negative for west. Null in 72 rows. |
| `coord_source` | string | DE 2016 school locations, or null when there is no point. |
| `source` | string | Department of Education NI, Institution Search (Schools Plus), OGL v3.0. |
| `updated` | string | Date the source list was read, 2026-11-10. |

## Data notes

* Map points come only from the Department of Education School Locations file published in February 2016, joined on the DE reference number. 1,044 of 1,116 schools (93.5%) have a point. The other 72 are null:
  * 53 schools are not in the 2016 file: all 15 independent schools, all 10 colleges and 28 newer or renumbered schools.
  * 19 schools have moved since 2016. Their 2016 address and postcode both differ from today's, so the old point was left out.
  * Five schools whose postcode changed but whose street address did not keep their 2016 point.
* Points are ten years old. A school may have moved within its site, or changed its address without a new DE reference number, since then.
* No extra map points were added from postcode centroids, Ordnance Survey, Land & Property Services or OpenStreetMap. The origin of the DE file’s points is not stated.
* Eight email addresses on webmail or with a person's name were removed. One mobile phone number was removed. No field holds a person's name.
* Phones were set to UK format. Four local numbers got the 028 area code. Second numbers were dropped.
* The source has no enrolment, ward or council fields. Status and date closed were dropped, since every row is open.
* Changes from the source: field names, whitespace cleanup, title case for all caps addresses, towns and counties, one space in postcodes, commas for semicolons, straight apostrophes, IS numbers closed up (IS 106 to IS106), blank for the colleges' UNKNOWN reference, phone format, emails and one mobile removed, added `lat`, `lng` and `coord_source`.

## Examples

Schools in Antrim whose names start with Gaelscoil. This call returns 6 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_ni_schools' --data-urlencode county=Antrim --data-urlencode 'name=Gaelscoil*'
```

First record:

```json
[
  {
    "de_ref": "104-6571",
    "name": "Gaelscoil na BhFál",
    "school_type": "Primary",
    "management_type": "Other Maintained",
    "address_line1": "34A Iveagh Crescent",
    "address_line2": null,
    "address_line3": null,
    "town": "Belfast",
    "county": "Antrim",
    "postcode": "BT12 6AW",
    "phone": "028 9023 6877",
    "email": "info@gaelscoilnabhfal.belfast.ni.sch.uk",
    "lat": 54.58999,
    "lng": -5.96154,
    "coord_source": "DE 2016 school locations",
    "source": "Department of Education NI, Institution Search (Schools Plus), OGL v3.0",
    "updated": "2026-11-10"
  }
]
```

Schools in a Belfast box. This call returns 147 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_ni_schools' --data-urlencode county=Antrim --data-urlencode lat=54.55..54.65 --data-urlencode lng=-6..-5.85
```

Grammar schools in Down. This call returns 13 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_ni_schools' --data-urlencode county=Down --data-urlencode school_type=Grammar
```

## Data source

[Department of Education (Northern Ireland), Institution Search (Schools Plus)](https://apps.education-ni.gov.uk/appinstitutes/default.aspx), open schools, read 10 November 2026.

Map points: [Department of Education, School Locations](https://admin.opendatani.gov.uk/dataset/locate-a-school) on OpenDataNI, file dated February 2016 (resource updated 3 July 2017). The dataset is labelled Open Government Licence v3.0.

Contains public sector information licensed under the [Open Government Licence v3.0 (OGL v3)](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/). Credit: Department of Education, see the [Department's Crown copyright page](https://www.education-ni.gov.uk/articles/crown-copyright-education). You may copy, share, adapt and use it for business if you credit the source and link the license. Logos and personal data are not covered. The license gives no warranty, and this API makes no claim of official status or endorsement by the Department.

The Department's Crown copyright page says that mapping branded by Ordnance Survey of Northern Ireland (Land & Property Services), or based on or derived from it, is outside the OGL. The School Locations dataset is published by the Department on OpenDataNI with an explicit OGL v3 label. It does not say how its points were made.
