# Singapore Private Education Institutions

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 319](https://img.shields.io/badge/records-319-lightgrey) ![updated 2026-10-30](https://img.shields.io/badge/updated-2026--10--30-orange) ![license facts and SODL v1.0](https://img.shields.io/badge/license-facts%20%2B%20SODL%20v1.0-green)

Every private education institution (PEI) registered with SkillsFuture Singapore (SSG), 319 schools as of October 2026. Each row has the school name, UEN, address, postal code, URA planning area and region, map point, website, first registration date, the start, end and length of the current registration, and the EduTrust status and end date. Filter by the required `edutrust_status`, then by name, postal code, area, registration dates or a coordinate range.

Rows by EduTrust status: None 178, Certified 110, Provisional 28 and Star 3. Rows by registration length: 4 years 177, 2 years 72 and 6 years 68.

> **Use of this data.** School names, registration and EduTrust facts come from the SSG PEI Listing (facts only). Not affiliated with or endorsed by SkillsFuture Singapore. Check a school's status with SSG before you enroll or pay fees.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/sg_private_education_institutions` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`edutrust_status`). Optional: 13 fields. Disallowed: 4 fields.

`edutrust_status` must be in every call. Use `edutrust_status=*` to list all 319 schools. Disallowed means the field is returned in the output but cannot be used as a filter.

Dates are text in the form `YYYY-MM-DD`, so filter them with a prefix wildcard, such as `2027*` for a year or `2027-03*` for a month. Only `latitude` and `longitude` take ranges.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `edutrust_status` | Required | Yes | No | EduTrust quality certification held now: `Star` (3), `Certified` (110), `Provisional` (28) or `None` (178). Star and Certified awards last 4 years, Provisional 1 year. `None` means no EduTrust award. Wildcards work, so `edutrust_status=*` lists all 319. |
| `name` | Optional | Yes | No | School name as SSG lists it, such as `*ACADEMY*` (70) or `Kaplan*` (1). |
| `uen` | Optional | Yes | No | The registration number SSG shows for the school, which is its Unique Entity Number (UEN), such as `200405777C`. One per row. |
| `address` | Optional | Yes | No | Street address of the registered premises, such as `*Orchard*` (12). Blank for 4 schools that SSG lists with no address. |
| `postal_code` | Optional | Yes | No | Six digit postal code, kept as text so leading zeros stay, such as `059817`, or `05*` (23) for a postal district. Blank for 4 rows. |
| `planning_area` | Optional | Yes | No | URA Master Plan 2019 planning area the map point falls in, such as `Downtown Core` (51) or `Tampines` (9). 35 areas are used. Blank for 4 rows. |
| `region` | Optional | Yes | No | URA region: `Central` (244), `West` (32), `North East` (17), `East` (16) or `North` (6). Blank for 4 rows. |
| `latitude` | Optional | No | Yes | Latitude of the premises in decimal degrees (WGS 84), from 1.24707 to 1.445562. Example: `latitude=1.28..1.31` (184). |
| `longitude` | Optional | No | Yes | Longitude of the premises in decimal degrees (WGS 84), from 103.682658 to 103.969781. Example: `longitude=103.84..103.86` (146). |
| `geocode_quality` | Optional | Yes | No | How the map point was placed: `postal_code` (309), `postal_code_corrected` (4), `school_website` (2) or `none` (4). See the notes. |
| `first_registered` | Optional | Yes | No | Date the school was first registered with the regulator, such as `2010*` (102) for a year. |
| `registration_start` | Optional | Yes | No | Start date of the current registration, such as `2026*` (89). |
| `registration_expiry` | Optional | Yes | No | Date the current registration ends, such as `2027*` (105) for every school whose registration ends in 2027. |
| `registration_period_years` | Optional | Yes | No | Length of the current registration in whole years, worked out from the start and end dates: `4` (177), `2` (72) or `6` (68). Blank for 2 rows whose dates do not span a whole number of years. |
| `website` | Disallowed | No | No | School website (https), as listed by SSG. Blank for 42 rows. |
| `edutrust_expiry` | Disallowed | No | No | Date the current EduTrust award ends, as `YYYY-MM-DD`. Blank when `edutrust_status` is `None` (178 rows). |
| `source` | Disallowed | No | No | Source of the row. |
| `source_as_of` | Disallowed | No | No | Date the source list was read, `2026-10-30` on every row. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `name` | string | School name as SSG lists it. |
| `uen` | string | Unique Entity Number. |
| `address` | string | Street address of the registered premises. Null for 4 rows. |
| `postal_code` | string | Six digit postal code, as text. Null for 4 rows. |
| `planning_area` | string | URA Master Plan 2019 planning area. Null for 4 rows. |
| `region` | string | URA region. Null for 4 rows. |
| `latitude` | number | Latitude in decimal degrees (WGS 84). Null for 4 rows. |
| `longitude` | number | Longitude in decimal degrees (WGS 84). Null for 4 rows. |
| `geocode_quality` | string | How the map point was placed. |
| `website` | string | School website (https). Null for 42 rows. |
| `first_registered` | string | Date first registered, `YYYY-MM-DD`. |
| `registration_start` | string | Start of the current registration, `YYYY-MM-DD`. |
| `registration_expiry` | string | End of the current registration, `YYYY-MM-DD`. |
| `registration_period_years` | string | Length of the current registration in years, as text. Null for 2 rows. |
| `edutrust_status` | string | `Star`, `Certified`, `Provisional` or `None`. |
| `edutrust_expiry` | string | End of the current EduTrust award, `YYYY-MM-DD`. Null for 178 rows. |
| `source` | string | Source of the row. |
| `source_as_of` | string | Date the source list was read. |

Notes:

* Registration length: the 4 year terms (177 rows) were given under the older rules. Since January 1, 2026, SSG gives 2 or 6 year terms, so a school moves to one of those when it renews.
* `geocode_quality`:
  * `postal_code` (309): the OneMap point for the postal code SSG gives.
  * `postal_code_corrected` (4): SSG's postal code was missing a leading zero or had a typo, so the code was checked against the street address in OneMap.
  * `school_website` (2): SSG shows no address, so the address came from the school's own website (ICAS Training and Education College and Kimage Hairdressing School).
  * `none` (4): SSG shows no address and none was found, so `address`, `postal_code`, `planning_area`, `region` and the map point are blank (Auston Institute of Management, Hitmaker Global Academy, Kenko Academy and SFA Eduhearts International School).
* Planning area and region come from the URA Master Plan 2019 boundary the map point falls in, not from the school.
* The 3 EduTrust Star schools are Global Indian International School, James Cook University and Singapore Institute of Management.
* Registration and EduTrust status change often. This is a snapshot from 2026-10-30. Check a school's status with SSG before you enroll or pay fees.

## Examples

A name wildcard. The full row for 3DSENSE Media School, whose postal code keeps its leading zero. This call returns 1 row:

```bash
curl -sG "https://api.jael.ee/JLEE/sg_private_education_institutions" --data-urlencode "edutrust_status=Certified" --data-urlencode "name=3DSENSE*"
```

```json
[
  {
    "__id": 1,
    "name": "3DSENSE MEDIA SCHOOL",
    "uen": "200405777C",
    "address": "6 EU TONG SEN STREET, #03-38, THE CENTRAL, Singapore 059817",
    "postal_code": "059817",
    "planning_area": "Singapore River",
    "region": "Central",
    "latitude": 1.288839,
    "longitude": 103.846558,
    "geocode_quality": "postal_code",
    "website": "https://www.3dsense.net",
    "first_registered": "2010-09-16",
    "registration_start": "2026-09-14",
    "registration_expiry": "2032-09-13",
    "registration_period_years": "6",
    "edutrust_status": "Certified",
    "edutrust_expiry": "2027-03-01",
    "source": "SSG PEI Listing, https://www.tpgateway.gov.sg/resources/information-for-private-education-institutions-(peis)/pei-listing; map point from OneMap; planning area from URA Master Plan 2019 (data.gov.sg)",
    "source_as_of": "2026-10-30"
  }
]
```

A prefix wildcard on a date. Schools with Provisional EduTrust whose registration ends in 2026. This call returns 1 row:

```bash
curl -sG "https://api.jael.ee/JLEE/sg_private_education_institutions" --data-urlencode "edutrust_status=Provisional" --data-urlencode "registration_expiry=2026*" | jq -r '.[] | [.name, .registration_expiry, .registration_period_years, .edutrust_expiry] | @tsv'
```

```text
UNITED CERES COLLEGE	2026-12-08	4	2027-07-17
```

A latitude and longitude range. Schools of any EduTrust status in a small box around Orchard Road. This call returns 11 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/sg_private_education_institutions" --data-urlencode "edutrust_status=*" --data-urlencode "latitude=1.30..1.31" --data-urlencode "longitude=103.83..103.84" | jq -r '.[] | [.name, .postal_code, .planning_area, .edutrust_status] | @tsv'
```

```text
AMITY GLOBAL INSTITUTE	238853	Orchard	Provisional
BIZ IQ INSTITUTE	238843	Orchard	None
CANADIAN EDUCATION COLLEGE	238869	Orchard	Certified
Connect2Excel International School	238164	Orchard	None
FUREN INTERNATIONAL SCHOOL	229572	Orchard	Certified
MAGES INSTITUTE OF EXCELLENCE	237978	Orchard	Certified
NGEE ANN ACADEMY	238873	Orchard	Certified
RAFFLES COLLEGE OF HIGHER EDUCATION	238164	Orchard	Certified
SEIMPI ACADEMY	238843	Orchard	None
SINGAPORE SPA INSTITUTE	238866	Orchard	None
Sophia Ignite Learning Academy	228213	Orchard	None
```

## Data source

School names, UENs, addresses, websites, registration dates and EduTrust facts come from the SkillsFuture Singapore (SSG) PEI Listing, read on 2026-10-30. Only facts were taken. Map points come from OneMap lookups of each postal code. Planning areas and regions come from the URA Master Plan 2019 Planning Area Boundary on data.gov.sg. Registration length and geocode quality are set by us, and the data can go out of date. Check a school's status with SSG before you enroll or pay fees.

Credit lines:

* School names, registration and EduTrust facts: SkillsFuture Singapore (SSG), PEI Listing (https://www.tpgateway.gov.sg/resources/information-for-private-education-institutions-(peis)/pei-listing), facts only.
* Planning areas and regions: URA Master Plan 2019 Planning Area Boundary, from data.gov.sg, licensed under the Singapore Open Data Licence version 1.0 (https://data.gov.sg/open-data-licence).
* Map points: OneMap, Singapore Land Authority (https://www.onemap.gov.sg).
* Not affiliated with or endorsed by SkillsFuture Singapore. Check a school's status with SSG before you enroll or pay fees.

[Back to the API list](../../README.md)
