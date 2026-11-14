# Northern Ireland Primary Care Practices

![status live](https://img.shields.io/badge/status-live-brightgreen) ![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 1,421](https://img.shields.io/badge/records-1%2C421-lightgrey) ![updated 2026-09-30](https://img.shields.io/badge/updated-2026--09--30-orange) ![license OGL v3](https://img.shields.io/badge/license-OGL%20v3-green)

1,421 primary care practices in Northern Ireland from official Business Services Organisation lists on Open Data NI: 304 GP practices as at the start of July 2026 (2,080,799 registered patients), 347 dental surgeries as at 30 September 2026, 264 ophthalmic practices as at 30 September 2026, and 506 community pharmacies from the September 2026 dispensing file. Each record has the service type, official practice code where one exists, name, address, postcode, phone where published, local commissioning group or council where published, and GP registered patient counts. No field is required, so you can filter by service type, town, postcode, name, council or a GP list size range.

GP, dental and optical lists are updated about every quarter, and the pharmacy source about every month. Partner names, individual dentist lists and person-only contractor names were omitted; official business names were kept. UK mobile numbers were omitted. There are no map points, because the official files have none.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/uk_ni_primary_care_practices` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per practice. All matches are returned. |

## Query parameters

Required: none. Optional: 14 fields. Disallowed: 2 fields.

A call with no filters returns all 1,421 records. Partial filters accept `*`, such as `name=*Boots*`, `town=*Derry*` or `phone=028 90*`. `service_type` and `dentist_type` are exact matches. `registered_patients` accepts a range in the form `min..max`, for example `registered_patients=10000..18926`. Text filters ignore case. `source` and `source_as_of` cannot be filtered.

Disallowed fields are returned but cannot be used as filters. A filter on a disallowed field returns HTTP 400. Filters are combined with AND. See [How filters work](../../README.md#how-filters-work).

| Field | Filter | Partial | Range | Description |
|---|---|---|---|---|
| `service_type` | Optional | No | No | Kind of practice. Exact match. Values: `gp` (304), `dental` (347), `optical` (264, official ophthalmic surgeries) or `pharmacy` (506). Never blank. |
| `practice_code` | Optional | Yes | No | Official code as text, with no added leading zeros. Present on all 304 GP rows (PracNo, such as 1) and all 506 pharmacy rows (Chemist contractor code, such as 3182). Blank (`null`) on all dental and optical rows, because those official lists have no practice code. The number is unique within a service type, but 20 codes appear on both a GP practice and a pharmacy, so callers should also filter by `service_type`. Partial wildcards allowed, such as `31*` (15) or `practice_code=1` (1). |
| `name` | Optional | Yes | No | Practice or business name. Present on all 304 GP rows, 75 dental rows, 242 optical rows and 487 pharmacy rows, so 313 rows have no name (`null`): 272 dental surgeries were published as an address only, and 22 optical and 19 pharmacy names were person-only contractor or directory listings and were left blank. Official business names were kept. Partial wildcards allowed, such as `*Boots*` (35) or `*Specsavers*` (26). |
| `address_line1` | Optional | Yes | No | First address line, usually the street or building. Never blank. Partial wildcards allowed, such as `*Health Centre*` (55). |
| `address_line2` | Optional | Yes | No | Second address line, often the street, building or town. Blank (`null`) on 34 rows. Partial wildcards allowed. |
| `address_line3` | Optional | Yes | No | Third address line. Blank (`null`) on 1,285 rows. Partial wildcards allowed. |
| `town` | Optional | Yes | No | Town or city taken from the official address when it was clearly present, in title case, such as Belfast (304) or Newry (48). Blank (`null`) on 100 rows where the source address had no usable town (93 GP, 6 dental and 1 optical). `town=*Derry*` matches Londonderry (58) and Derry (10), together 71. Partial wildcards allowed. |
| `postcode` | Optional | Yes | No | Postcode as text, with one space, such as BT4 1NS. Present on every row. Two official values are defective: BL4 7NS (an English postcode on one optical practice in Bolton) and BT15 (an incomplete pharmacy postcode). A prefix of `BT4*` also matches BT40 to BT49 (277). Partial wildcards allowed, such as `BT48*` (55). |
| `phone` | Optional | Yes | No | Practice landline in UK format, such as 028 9060 3440. Present on 345 dental and 262 optical rows. All 304 GP and 506 pharmacy rows are blank (`null`) because those official files have no phone. UK mobile numbers were omitted; two optical rows listed only a mobile and are blank, and mixed numbers kept only the landline. Two dental numbers were published as NULL and are blank. Partial wildcards allowed, such as `028 90*` (155) or `028*` (604). |
| `lcg` | Optional | Yes | No | Local commissioning group, from the official file, in five values: Northern (218), Belfast (205), Southern (179), South Eastern (163) and Western (150). Present on all GP, dental and optical rows. Blank (`null`) on all 506 pharmacy rows, because that file has no LCG. The GP list notes that practices 473 (Moira Surgery) and 475 (Aghalee Surgery) sit in South Eastern by postcode but are managed by the Southern Trust LCG; the published value South Eastern was kept. Partial wildcards allowed, such as `*Eastern*` (163, South Eastern). |
| `lgd` | Optional | Yes | No | Local government district (council), from the official dental and optical files, in 11 values. The largest are Belfast (125) and Newry, Mourne and Down (64). Blank (`null`) on all GP and pharmacy rows, because those files have no council field. Partial wildcards allowed, such as `*Ulster*` (46) or `lgd=Mid Ulster` (46). |
| `assembly_area` | Optional | Yes | No | Northern Ireland Assembly area, from the official dental file only, such as Belfast South (30). Blank (`null`) on GP, optical and pharmacy rows. Partial wildcards allowed, such as `Belfast*` (93). |
| `dentist_type` | Optional | No | No | Dental contract type from the official file: `gds` (346) or `salaried` (1). Blank (`null`) on all non-dental rows. Exact match. |
| `registered_patients` | Optional | No | Yes | Registered patient list size as an integer, from the official GP file, ranging from 1,349 to 18,926. Present on all 304 GP rows, summing to 2,080,799. Blank (`null`) on all other types. Use a range, for example `registered_patients=10000..18926` (39) or `registered_patients=15000..18926` (4). A hyphen range such as `10000-18926` returns HTTP 400. |
| `source` | Disallowed | No | No | Attribution for the official BSO list that the row comes from, on every row, under the Open Government Licence v3.0. Not filterable. |
| `source_as_of` | Disallowed | No | No | As-at date of that official list, as text YYYY-MM-DD. GP rows are `2026-07-01` (list sizes at the start of the July 2026 quarter). Dental, optical and pharmacy rows are `2026-09-30` (surgeries as at 30 September 2026, and the September 2026 dispensing month). Not filterable. |

## Response fields

`__id` and `registered_patients` are integers. All other values are strings. Blank cells are returned as `null`. `__id` is the stable row key, because no official code is unique across all four lists (dental and optical have no code, and 20 GP and pharmacy codes overlap). Callers should use `service_type` plus `practice_code`, or name and postcode, to identify a practice.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `service_type` | string | Kind of practice: `gp`, `dental`, `optical` or `pharmacy`. |
| `practice_code` | string | Official GP or pharmacy code as text, or `null` on dental and optical rows. |
| `name` | string | Practice or business name, or `null` when the official list had no usable name. |
| `address_line1` | string | First address line. |
| `address_line2` | string | Second address line, or `null`. |
| `address_line3` | string | Third address line, or `null`. |
| `town` | string | Town or city in title case, or `null` when the source address had no usable town. |
| `postcode` | string | Postcode as text, with one space. |
| `phone` | string | Practice landline in UK format, or `null` when no landline was published. |
| `lcg` | string | Local commissioning group, or `null` on pharmacy rows. |
| `lgd` | string | Local government district (council), or `null` on GP and pharmacy rows. |
| `assembly_area` | string | Northern Ireland Assembly area, or `null` on non-dental rows. |
| `dentist_type` | string | Dental contract type, `gds` or `salaried`, or `null` on non-dental rows. |
| `registered_patients` | integer | GP registered patient list size, or `null` on non-GP rows. |
| `source` | string | Contains public sector information from the Business Services Organisation, published on Open Data NI, licensed under the Open Government Licence v3.0. Then the name of the official list for that row. |
| `source_as_of` | string | As-at date of that official list: `2026-07-01` for GP rows, `2026-09-30` for dental, optical and pharmacy rows. |

## Examples

Harland Medical Practice in Belfast, looked up by official GP practice code 1. This call returns 1 record:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_ni_primary_care_practices' --data-urlencode 'practice_code=1'
```

```json
[
  {
    "__id": 1,
    "service_type": "gp",
    "practice_code": "1",
    "name": "Harland Medical Practice",
    "address_line1": "Holywood Arches Health Centre",
    "address_line2": "Westminster Avenue",
    "address_line3": null,
    "town": null,
    "postcode": "BT4 1NS",
    "phone": null,
    "lcg": "Belfast",
    "lgd": null,
    "assembly_area": null,
    "dentist_type": null,
    "registered_patients": 6740,
    "source": "Contains public sector information from the Business Services Organisation, published on Open Data NI, licensed under the Open Government Licence v3.0. GP Practice Reference File, July 2026 (list sizes at the start of the quarter)",
    "source_as_of": "2026-07-01"
  }
]
```

GP practices. This call returns 304 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_ni_primary_care_practices' --data-urlencode 'service_type=gp'
```

Practices whose name includes Boots. This call returns 35 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_ni_primary_care_practices' --data-urlencode 'name=*Boots*'
```

GP practices with 10,000 to 18,926 registered patients. This call returns 39 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_ni_primary_care_practices' --data-urlencode 'registered_patients=10000..18926'
```

GP practices in the Western local commissioning group. This call returns 47 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_ni_primary_care_practices' --data-urlencode 'service_type=gp' --data-urlencode 'lcg=Western'
```

Towns matching *Derry*, including Londonderry and Derry. This call returns 71 records:

```bash
curl -sS -A 'Mozilla/5.0' -G 'https://api.jael.ee/JLEE/uk_ni_primary_care_practices' --data-urlencode 'town=*Derry*'
```

## Data source

Source: Business Services Organisation lists published on Open Data NI, used under the [Open Government Licence v3.0](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/). The four official lists are the [GP Practice Reference File, July 2026](https://www.opendatani.gov.uk/dataset/gp-practice-list-sizes) (list sizes at the start of the July 2026 quarter), the [Dental Surgery List as at 30 September 2026](https://www.opendatani.gov.uk/dataset/general-dental-services-statistics), the [Ophthalmic Surgery List as at 30 September 2026](https://www.opendatani.gov.uk/dataset/ophthalmic-surgery-list), and community pharmacies taken from unique contractors in [Dispensed Items by GP and Pharmacy, September 2026](https://www.opendatani.gov.uk/dataset/dispensing-by-contractor). From 514 contractors in that dispensing file, 6 dispensing-doctor rows and 2 non-pharmacy contractors (a wholesaler and an appliance supplier) were left out, leaving 506 pharmacies. The separate Dentist Contractor List was not used. GP, dental and optical lists are updated about every quarter, and the pharmacy source about every month.

Contains public sector information from the Business Services Organisation, published on Open Data NI, licensed under the Open Government Licence v3.0.

The data is used under the [Open Government Licence v3.0](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/). See also the [Open Data NI terms](https://www.opendatani.gov.uk/terms-and-conditions). You may copy, share, adapt and use the information for any purpose, including commercial use, if you use that credit and link to the licence. If those conditions are not met, the rights granted under the licence end automatically. The licence does not cover personal data, or departmental or public sector logos, crests and the Royal Arms. The data comes with no warranty. This licence does not grant any right to use the information in a way that suggests official status or that the Business Services Organisation or Open Data NI endorses this API or its maker.

Partner names, individual dentist lists and person-only contractor names were omitted; official business names were kept. UK mobile numbers were omitted. There are no map points. One optical row, Eyecare For You, is published with an English address and postcode BL4 7NS in Farnworth, Bolton. One pharmacy postcode is only BT15. The official address 173 Anderstownstown Road is a typo for Andersonstown, and one optical address uses Ennskillen for Enniskillen. 272 dental rows have no practice name in the source. 20 `practice_code` values appear on both a GP practice and a pharmacy.

This is a snapshot of those official lists (GP as at the start of July 2026; dental, optical and pharmacy as at 30 September 2026). For the latest files, visit the dataset pages above.

[Back to the API list](../../README.md)
