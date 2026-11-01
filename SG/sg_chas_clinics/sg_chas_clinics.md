# Singapore CHAS Clinics

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 1193](https://img.shields.io/badge/records-1193-lightgrey) ![updated 2024-06-06](https://img.shields.io/badge/updated-2024--06--06-orange) ![license SODL v1.0](https://img.shields.io/badge/license-SODL%20v1.0-green)

All 1,193 GP and dental clinics on the Ministry of Health (MOH) list of Community Health Assist Scheme (CHAS) clinics, from data.gov.sg. Each row has the clinic name, MOH code, licence type, scheme codes, phone, block, street, floor, unit, building, postal code, full address, URA planning area and region, and map point. Filter by the required `planning_area`, then by name, postal code, scheme code or a coordinate range.

Rows by scheme codes: CDMP,CHAS 574, CDMP,CHAS,ISP 475 and CHAS 144. Rows by region: Central 431, West 222, North-East 221, East 194 and North 125.

> **Data age.** Every row is dated 2021-09-26 by MOH, and the source was last updated on 2024-06-06. Clinics open, close and move, so check with the clinic or the CHAS website before you visit.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/sg_chas_clinics` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`planning_area`). Optional: 15 fields. Disallowed: 3 fields.

`planning_area` must be in every call. Use `planning_area=*` to list all 1,193 clinics. Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `planning_area` | Required | Yes | No | URA Master Plan 2019 planning area the clinic point falls in. 42 areas are used, such as `Tampines` (81), `Bedok` (79), `Ang Mo Kio` (64), `Yishun` (61), `Jurong West` (60), `Bukit Merah` (59), `Hougang` (55), `Toa Payoh` (50), `Woodlands` (47), `Sengkang` (44), `Queenstown` (43) and `Downtown Core` (43). Use `planning_area=*` for all 1,193. |
| `hci_code` | Optional | Yes | No | MOH healthcare institution (HCI) code of the clinic, such as `15M0211`. One per row. |
| `clinic_name` | Optional | Yes | No | Clinic name as MOH lists it, such as `*Raffles*` (49) or `*Family Clinic*`. Case and spacing vary in the source, so use wildcards. |
| `licence_type` | Optional | Yes | No | MOH licence type code as published: `MC` (1053) or `MD` (140). The source does not explain the codes. |
| `programme_codes` | Optional | Yes | No | Scheme codes the clinic takes part in, a comma list as MOH writes it: `CDMP,CHAS` (574), `CDMP,CHAS,ISP` (475) or `CHAS` (144). CHAS is the Community Health Assist Scheme and CDMP the Chronic Disease Management Programme. The source does not explain ISP. Use wildcards, such as `*ISP*` (475). |
| `phone` | Optional | Yes | No | Clinic phone number, usually 8 digits with no country code (+65), such as `6244*`. Blank for 5 rows. |
| `block` | Optional | Yes | No | Block or house number, such as `123` or `201D`. Blank for 1 row. |
| `street_name` | Optional | Yes | No | Street name, such as `*Ang Mo Kio*` (63). ALL CAPS names were changed to title case. MOH short forms such as Ave, Rd and St are kept. |
| `floor` | Optional | Yes | No | Floor, kept as text, such as `01` (890) or `B1` (27). Blank for 59 rows. |
| `unit` | Optional | Yes | No | Unit number, such as `23` or `08/09`. Blank for 59 rows. |
| `building_name` | Optional | Yes | No | Building name as MOH writes it, such as `*Hub*` (5). Blank for 742 rows. |
| `postal_code` | Optional | Yes | No | Six digit postal code, kept as text so leading zeros stay, such as `059818` (2), or `56*` (63) for a postal district. |
| `address` | Optional | Yes | No | Full address built from block, street, #floor-unit, building and postal code, such as `*Tampines*` (67). |
| `region` | Optional | Yes | No | URA region of the planning area: `Central Region` (431), `West Region` (222), `North-East Region` (221), `East Region` (194) or `North Region` (125). |
| `latitude` | Optional | No | Yes | Latitude in decimal degrees (WGS 84), from 1.257039 to 1.456037. Example: `latitude=1.34..1.36` (228). |
| `longitude` | Optional | No | Yes | Longitude in decimal degrees (WGS 84), from 103.626129 to 103.990349. Example: `longitude=103.84..103.86` (235). |
| `coord_source` | Disallowed | No | No | Where the map point came from: `moh` (1192) is the point in the MOH file, `openstreetmap` (1) marks the one row whose MOH point was missing. |
| `record_updated` | Disallowed | No | No | Date MOH last updated the row, as `YYYY-MM-DD`: `2021-09-26` on all 1,193 rows. |
| `source_url` | Disallowed | No | No | Source dataset page on data.gov.sg. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.
* A call without `planning_area` returns a 400 error: `Your request is missing the following required parameters: planning_area. You did not supply any.`

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `hci_code` | string | MOH healthcare institution code. |
| `clinic_name` | string | Clinic name as MOH lists it. |
| `licence_type` | string | MOH licence type code, `MC` or `MD`. |
| `programme_codes` | string | Scheme codes, comma separated. |
| `phone` | string | Phone number, as text. Null for 5 rows. |
| `block` | string | Block or house number, as text. Null for 1 row. |
| `street_name` | string | Street name. |
| `floor` | string | Floor, as text. Null for 59 rows. |
| `unit` | string | Unit number, as text. Null for 59 rows. |
| `building_name` | string | Building name. Null for 742 rows. |
| `postal_code` | string | Six digit postal code, as text. |
| `address` | string | Full address. |
| `planning_area` | string | URA Master Plan 2019 planning area. |
| `region` | string | URA region. |
| `latitude` | number | Latitude in decimal degrees (WGS 84). |
| `longitude` | number | Longitude in decimal degrees (WGS 84). |
| `coord_source` | string | `moh` or `openstreetmap`. |
| `record_updated` | string | Date MOH last updated the row, `YYYY-MM-DD`. |
| `source_url` | string | Source dataset page on data.gov.sg. |

## Examples

A wildcard on scheme codes. Clinics in Ang Mo Kio that list ISP. This call returns 30 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/sg_chas_clinics" --data-urlencode "planning_area=Ang Mo Kio" --data-urlencode "programme_codes=*ISP*"
```

Response (30 rows, first row shown):

```json
[
  {
    "__id": 8,
    "hci_code": "16M0043",
    "clinic_name": "Ang Mo Kio Family Medicine Clinic",
    "licence_type": "MC",
    "programme_codes": "CDMP,CHAS,ISP",
    "phone": "65541133",
    "block": "4190",
    "street_name": "Ang Mo Kio Avenue 6",
    "floor": "03",
    "unit": "01",
    "building_name": "BROADWAY PLAZA",
    "postal_code": "569841",
    "address": "4190 Ang Mo Kio Avenue 6 #03-01 BROADWAY PLAZA Singapore 569841",
    "planning_area": "Ang Mo Kio",
    "region": "North-East Region",
    "latitude": 1.371937,
    "longitude": 103.845949,
    "coord_source": "moh",
    "record_updated": "2021-09-26",
    "source_url": "https://data.gov.sg/datasets/d_548c33ea2d99e29ec63a7cc9edcccedc/view"
  },
  ...
]
```

A postal code with a leading zero, in all planning areas. This call returns 2 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/sg_chas_clinics" --data-urlencode "planning_area=*" --data-urlencode "postal_code=059818" | jq -r '.[] | [.clinic_name, .postal_code, .planning_area, .programme_codes] | @tsv'
```

```text
1Aesthetics, Medical & Surgery	059818	Singapore River	CDMP,CHAS
Jade Medical Clinic Pte Ltd	059818	Singapore River	CDMP,CHAS
```

A latitude and longitude range. Clinics in a box around Bishan and Toa Payoh. This call returns 20 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/sg_chas_clinics" --data-urlencode "planning_area=*" --data-urlencode "latitude=1.34..1.36" --data-urlencode "longitude=103.84..103.86" | jq length
```

## Data source

Clinic names, codes, phone numbers, addresses and map points come from CHAS Clinics (Ministry of Health), on data.gov.sg ([dataset page](https://data.gov.sg/datasets/d_548c33ea2d99e29ec63a7cc9edcccedc/view)), last updated June 6, 2024. Planning areas and regions come from the URA Master Plan 2019 Planning Area Boundary (No Sea) on data.gov.sg. License: [Singapore Open Data Licence version 1.0](https://data.gov.sg/open-data-licence). It allows free use, including commercial use, as long as you credit the source and do not suggest the agency backs you.

Credit line: Contains information from CHAS Clinics (Ministry of Health, last updated 6 June 2024) accessed on 1 November 2026 from data.gov.sg (https://data.gov.sg/datasets/d_548c33ea2d99e29ec63a7cc9edcccedc/view), and from Master Plan 2019 Planning Area Boundary (No Sea) (Urban Redevelopment Authority) accessed on 1 November 2026 from data.gov.sg, which are made available under the terms of the Singapore Open Data Licence version 1.0 (https://data.gov.sg/open-data-licence).

Changes made: the address table in each source row was split into columns, street names were changed to title case, the full address was built, and planning areas were added from the map point. One map point comes from OpenStreetMap: (c) OpenStreetMap contributors, licensed under the [Open Database License (ODbL)](https://opendatacommons.org/licenses/odbl/). This API is not endorsed by the Ministry of Health. The rows date from 2021, so check a clinic before you rely on it.

[Back to the API list](../../README.md)
