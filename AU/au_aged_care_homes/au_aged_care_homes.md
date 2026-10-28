# Australia Aged Care Homes

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 2590](https://img.shields.io/badge/records-2590-lightgrey) ![updated 2026-10-28](https://img.shields.io/badge/updated-2026--10--28-orange) ![license CC BY 4.0](https://img.shields.io/badge/license-CC%20BY%204.0-green)

Government funded residential aged care homes across Australia, 2,590 services from the Department of Health, Disability and Ageing list at 30 June 2025. Each row has the home name, provider, organisation type, number of residential places, street address, suburb, state, postcode, remoteness, planning region, local government area, primary health network, 2024-25 funding and map coordinates. The list is a snapshot, so homes may have opened, closed or moved since.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/au_aged_care_homes` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`state`). Optional: 17 fields. Disallowed: 3 fields.

`state` must be in every call. Use `state=*` to list all 2,590 homes. Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `state` | Required | Yes | No | State or territory: `NSW` (817), `VIC` (733), `QLD` (457), `WA` (249), `SA` (233), `TAS` (65), `ACT` (27) or `NT` (9). Wildcards work, so `state=*` lists all 2,590 homes. |
| `service_name` | Optional | Yes | No | Home name, such as `*Mirinjani*` or `Dellacourt`. |
| `provider_name` | Optional | Yes | No | Organisation that runs the home, such as `*Uniting*` or `BaptistCare*`. 709 distinct providers. |
| `organisation_type` | Optional | No | No | Kind of provider: `Private Incorporated Body` (852), `Charitable` (661), `Religious` (527), `Community Based` (353), `State Government` (184), `Local Government` (12) or `Publicly Listed Company` (1). |
| `residential_places` | Optional | No | Yes | Government funded residential places (beds), a whole number from 4 to 337. Example: `residential_places=100..200`. |
| `address` | Optional | Yes | No | Street address, such as `*Main Street*`. 55 addresses have no street number. |
| `suburb` | Optional | Yes | No | Suburb in upper case, such as `BALLINA` or `CAPEL*`. 1,523 distinct suburbs. |
| `postcode` | Optional | Yes | No | Four digit postcode as text, so `0870` keeps its zero. Examples: `2640` or `20*`. |
| `remoteness` | Optional | No | No | ABS remoteness area: `Major Cities of Australia` (1,671), `Inner Regional Australia` (617), `Outer Regional Australia` (267), `Remote Australia` (26) or `Very Remote Australia` (9). |
| `mmm_code` | Optional | No | No | Modified Monash Model 2019 code, a whole number from 1 (metropolitan) to 7 (very remote): 1 (1,660), 2 (204), 3 (229), 4 (178), 5 (286), 6 (25), 7 (8). |
| `aged_care_planning_region` | Optional | Yes | No | 2018 Aged Care Planning Region, such as `Southern Metro`, `Hunter` or `*Sydney*`. 65 distinct regions. |
| `sa2_name` | Optional | Yes | No | ABS 2016 Statistical Area 2 name. 1,312 distinct values. |
| `sa3_name` | Optional | Yes | No | ABS 2016 Statistical Area 3 name. 323 distinct values. |
| `lga_name` | Optional | Yes | No | 2023 local government area name, such as `Albury` or `*Brisbane*`. 387 distinct values. |
| `phn_name` | Optional | Yes | No | 2017 Primary Health Network name, such as `Adelaide` or `*Melbourne*`. 31 distinct values. |
| `funding_2024_25_aud` | Optional | No | Yes | Australian Government funding for the service in 2024-25, in Australian dollars, from 0 to 35,484,730.14. Two homes show 0. Example: `funding_2024_25_aud=5000000..10000000`. |
| `latitude` | Optional | No | Yes | Latitude in decimal degrees (WGS 84), from -43.314254 to -10.569817. Blank for 1 home. Example: `latitude=-34..-33`. |
| `longitude` | Optional | No | Yes | Longitude in decimal degrees (WGS 84), from 114.60801 to 153.61942. Blank for 1 home. Example: `longitude=150..152`. |
| `geocode_quality` | Optional | No | No | `exact` (2,589), the Department's point for the service address, checked by us, or `none` (1), no safe point. |
| `care_type` | Disallowed | No | No | Always `Residential` in this table. |
| `source` | Disallowed | No | No | Data source and licence credit for the row. |
| `updated` | Disallowed | No | No | Date the data was pulled, such as `2026-10-28`. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `service_name` | string | Name of the aged care home as listed by the Department. |
| `provider_name` | string | Organisation that runs the home. |
| `organisation_type` | string | Kind of provider. |
| `care_type` | string | Always `Residential`. |
| `residential_places` | integer | Government funded residential places (beds). |
| `address` | string | Street address. |
| `suburb` | string | Suburb, upper case. |
| `state` | string | State or territory code. |
| `postcode` | string | Four digit postcode, as text. |
| `remoteness` | string | ABS remoteness area. |
| `mmm_code` | integer | Modified Monash Model code, 1 to 7. |
| `aged_care_planning_region` | string | 2018 Aged Care Planning Region. |
| `sa2_name` | string | ABS 2016 Statistical Area 2 name. |
| `sa3_name` | string | ABS 2016 Statistical Area 3 name. |
| `lga_name` | string | 2023 local government area name. |
| `phn_name` | string | 2017 Primary Health Network name. |
| `funding_2024_25_aud` | number | Australian Government funding in 2024-25, in Australian dollars. |
| `latitude` | number | Latitude in decimal degrees (WGS 84). Null for 1 home. |
| `longitude` | number | Longitude in decimal degrees (WGS 84). Null for 1 home. |
| `geocode_quality` | string | `exact` or `none`. |
| `source` | string | Where the row came from, with the credit line. |
| `updated` | string | Date the data was pulled. |

Notes: the source file has 5,378 service rows across 7 care types. This API keeps the 2,590 rows marked Residential, which hold 224,493 places in total. Home care, short term restorative care, transition care, Multi-Purpose Service and other care types are left out. Seven service names appear twice, and four of those pairs share an address but have different place counts, so both rows are kept. Mt Lofty Nursing Home in Queensland has no latitude or longitude because the source point is 25.6 km from its street. Thirteen homes labelled `Major Cities of Australia` have a `mmm_code` above 1, and one planning region is labelled `Cabool` in the source. Both are kept as the source has them. Coordinates are the Department's own points, checked against the state boundaries and a sample of 81 homes. Cleaning was limited to extra spaces, trailing dashes, upper case suburbs and four digit postcodes.

## Examples

Homes in the Northern Territory, listing the names. This call returns 9 rows:

```bash
curl -s "https://api.jael.ee/JLEE/au_aged_care_homes?state=NT" | jq -r '.[].service_name'
```

```text
Old Timers
Rocky Ridge
Juninga Centre
Terrace Gardens
Katherine Hostel
Hetti Perkins
Flynn Lodge
Regis Tiwi
Pearl Supported Care
```

One home by name in the Northern Territory:

```bash
curl -s "https://api.jael.ee/JLEE/au_aged_care_homes?state=NT&service_name=Hetti*"
```

Response (1 row):

```json
[
  {
    "__id": 1612,
    "service_name": "Hetti Perkins",
    "provider_name": "Australian Regional and Remote Community Services Limited",
    "organisation_type": "Community Based",
    "care_type": "Residential",
    "residential_places": 60,
    "address": "2 Percy Court",
    "suburb": "ALICE SPRINGS",
    "state": "NT",
    "postcode": "0870",
    "remoteness": "Remote Australia",
    "mmm_code": 6,
    "aged_care_planning_region": "Alice Springs",
    "sa2_name": "Ross",
    "sa3_name": "Alice Springs",
    "lga_name": "Alice Springs",
    "phn_name": "Northern Territory",
    "funding_2024_25_aud": 9635786.18,
    "latitude": -23.748953,
    "longitude": 133.874256,
    "geocode_quality": "exact",
    "source": "Department of Health, Disability and Ageing, Aged care service list 30 June 2025 (published on GEN aged care data by AIHW), CC BY 4.0",
    "updated": "2026-10-28"
  }
]
```

A wildcard on the provider. Victorian homes run by a provider with Uniting in its name. This call returns 13 rows, and the first three are Uniting AgeWell Condare Court, Uniting AgeWell Strath-Haven and Uniting AgeWell Kalkee Nangatta:

```bash
curl -s "https://api.jael.ee/JLEE/au_aged_care_homes?state=VIC&provider_name=*Uniting*" | jq -r '.[0:3][].service_name'
```

A range on places. Homes in the Australian Capital Territory with 100 to 200 places. This call returns 16 rows:

```bash
curl -s "https://api.jael.ee/JLEE/au_aged_care_homes?state=ACT&residential_places=100..200" | jq -r '.[].service_name'
```

A box on latitude and longitude. New South Wales homes around inner west Sydney. This call returns 224 rows, and the first three are Macquarie Lodge Aged Care Plus Centre, Ashfield Terrace Care Community and Weeroona Aged Care Plus Centre:

```bash
curl -s "https://api.jael.ee/JLEE/au_aged_care_homes?state=NSW&latitude=-34..-33.8&longitude=150.8..151.3" | jq -r '.[0:3][].service_name'
```

## Data source

The list comes from the Aged care service list at 30 June 2025, published by the Department of Health, Disability and Ageing on GEN aged care data (run by the Australian Institute of Health and Welfare). The file used was refreshed on 2026-01-30 and was downloaded on 2026-10-28. No newer edition was found. Coordinates are the Department's own points, so nothing was geocoded by us. The data is released under a [Creative Commons BY 4.0](https://creativecommons.org/licenses/by/4.0/) licence, as stated on the [GEN copyright page](https://www.gen-agedcaredata.gov.au/copyright).

Credit line: Based on Australian Institute of Health and Welfare material. Data: Aged care service list, 30 June 2025, Department of Health, Disability and Ageing, published on GEN aged care data (https://www.gen-agedcaredata.gov.au), licensed CC BY 4.0. Changes were made: residential aged care services only, text tidied, some columns left out. This does not imply endorsement by the Australian Institute of Health and Welfare or the Department.

[Back to the API list](../../README.md)
