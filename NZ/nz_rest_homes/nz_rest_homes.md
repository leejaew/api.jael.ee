# NZ Certified Rest Homes and Aged Care

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 665](https://img.shields.io/badge/records-665-lightgrey) ![licence CC BY 4.0](https://img.shields.io/badge/licence-CC%20BY%204.0-orange)

All 665 certified aged care premises in New Zealand, from the Ministry of Health HealthCert list. Each row is one rest home or care facility with its name, operator, certified service types (such as rest home, geriatric, medical or dementia care), total beds, address, Health New Zealand district and region, certificate end date, auditor and map coordinates. The data covers 20 districts and 140 towns and cities.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/nz_rest_homes` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field. Optional: 14 fields. Disallowed: 2 fields.

`district` is required. Use `district=*` to search all districts. Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `district` | Required | Yes | No | Health New Zealand district, mapped from the old district health board, spelled without macrons: `Canterbury` (85), `Waitemata` (72), `Auckland` (70), `Southern` (59), `Waikato` (58), `Capital, Coast and Hutt Valley` (46), `Counties Manukau` (46), `Bay of Plenty` (37), `MidCentral` (33), `Hawke's Bay` (27), `Taranaki` (26), `Northland` (26), `Nelson Marlborough` (24), `Lakes` (13), `Wairarapa` (12), `Whanganui` (11), `South Canterbury` (11), `Tairawhiti` (5), `West Coast` (4). Use `district=*` for all 665. |
| `premises_name` | Optional | Yes | No | Name of the rest home or care facility, for example `*Ryman*` or `*Lodge*`. |
| `legal_name` | Optional | Yes | No | Company or trust that holds the certificate, for example `*Bupa*` or `*Oceania*`. |
| `service_types` | Optional | Yes | No | Certified service types as a comma list, as the Ministry writes them. Use wildcards, such as `*Dementia*`. Counts: Rest home care (619), Geriatric (497), Medical (481), Dementia care (249), Physical (70), Psychogeriatric (33), Intellectual (18), Sensory (7), Psychiatric (7), Maternity (4), Mental health (1). |
| `total_beds` | Optional | No | Yes | Total certified beds, from 5 to 284. Use a range, for example `total_beds=50..100`. |
| `address` | Optional | Yes | No | Street address of the premises, for example `*Grey Street*`. |
| `suburb` | Optional | Yes | No | Suburb or locality, such as `Henderson`. Blank for 209 rows. |
| `town_city` | Optional | Yes | No | Town or city, such as `Christchurch` or `Hamilton`. |
| `postcode` | Optional | Yes | No | 4 digit New Zealand postcode as text, such as `0610` or `80*`. Leading zeros are kept. |
| `health_nz_region` | Optional | Yes | No | Health New Zealand region: `Northern` (214), `South Island` (183), `Midland` (139), `Central` (129). |
| `dhb_name` | Optional | Yes | No | District health board name as the Ministry still lists it, such as `Waitemata District Health Board`. |
| `cert_end_date` | Optional | No | Yes | Date the current certificate ends, from 2026-10-31 to 2030-11-12. Use a date range, for example `cert_end_date=2028-01-01..2030-12-31`. |
| `auditor` | Optional | Yes | No | Designated auditing agency that last audited the premises: `BSI Group New Zealand Ltd` (499), `The DAA Group Limited` (151), `Quality Health Authority` (15). |
| `lat` | Optional | No | Yes | Latitude in decimal degrees (WGS84), from -46.418136 to -35.109481. Use a range to search a box, for example `lat=-43.6..-43.4`. |
| `long` | Optional | No | Yes | Longitude in decimal degrees (WGS84), from 168.023376 to 178.304945. Use a range to search a box, for example `long=172.5..172.8`. |
| `geocode_precision` | Disallowed | No | No | How the map point was found in OpenStreetMap: `address` (641), `street` (14), `premises` (8), `suburb` (2). |
| `source_url` | Disallowed | No | No | Source file of the row. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.
* A call without `district` returns a 400 error: `Your request is missing the following required parameters: district.`

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `premises_name` | string | Name of the rest home or care facility. |
| `legal_name` | string | Company or trust that holds the certificate. |
| `service_types` | string | Certified service types, comma separated. |
| `total_beds` | integer | Total certified beds. |
| `address` | string | Street address. |
| `suburb` | string, null | Suburb or locality. Null when the Ministry list has none. |
| `town_city` | string | Town or city. |
| `postcode` | string | 4 digit New Zealand postcode. |
| `district` | string | Health New Zealand district. |
| `health_nz_region` | string | Health New Zealand region. |
| `dhb_name` | string | District health board name as listed by the Ministry. |
| `cert_end_date` | string | Date the current certificate ends, as a date and time, such as `2028-06-30T00:00:00+00:00`. |
| `auditor` | string | Designated auditing agency. |
| `lat` | number | Latitude in decimal degrees. |
| `long` | number | Longitude in decimal degrees. |
| `geocode_precision` | string | `address` is the street number, `premises` is the named facility, `street` is a point on the street, `suburb` is the suburb center. |
| `source_url` | string | Source file of the row. |

## Examples

Dementia care premises in the Canterbury district, using a wildcard on service type. This call returns 34 rows:

```bash
curl -s "https://api.jael.ee/JLEE/nz_rest_homes?district=Canterbury&service_types=*Dementia*"
```

Response (34 rows, first row shown):

```json
[
  {
    "__id": 673,
    "premises_name": "Adriel Resthome",
    "legal_name": "Adriel Rest Home Limited",
    "service_types": "Dementia care",
    "total_beds": 42,
    "address": "36 Osborne Road",
    "suburb": null,
    "town_city": "Amberley",
    "postcode": "7410",
    "district": "Canterbury",
    "health_nz_region": "South Island",
    "dhb_name": "Canterbury District Health Board",
    "cert_end_date": "2028-06-30T00:00:00+00:00",
    "auditor": "The DAA Group Limited",
    "lat": -43.147421,
    "long": 172.733168,
    "geocode_precision": "address",
    "source_url": "https://www.health.govt.nz/system/files/LegalEntitySummaryAgedCare.csv"
  },
  ...
]
```

Larger dementia care premises in the Southern district, using a range on beds. This call returns 3 rows:

```bash
curl -s "https://api.jael.ee/JLEE/nz_rest_homes?district=Southern&service_types=*Dementia*&total_beds=100..200" | jq -r '.[] | "\(.premises_name) | \(.town_city) | \(.total_beds)"'
```

```text
Ascot Care Home | Invercargill | 104
Peacehaven Village | Invercargill | 126
Rowena Jackson Retirement Village | Invercargill | 169
```

All districts, certificates that end from 2028 to 2030, using a date range. This call returns 411 rows:

```bash
curl -s "https://api.jael.ee/JLEE/nz_rest_homes?district=*&cert_end_date=2028-01-01..2030-12-31" | jq length
```

Premises in central Wellington, using a box of latitude and longitude ranges. This call returns 15 rows:

```bash
curl -s "https://api.jael.ee/JLEE/nz_rest_homes?district=*&lat=-41.35..-41.2&long=174.7..174.85" | jq length
```

## Data source

Ministry of Health, New Zealand: HealthCert list of certified rest home providers ([LegalEntitySummaryAgedCare.csv](https://www.health.govt.nz/system/files/LegalEntitySummaryAgedCare.csv), updated 30 October 2026, from the [certified rest homes page](https://www.health.govt.nz/regulation-legislation/certification-of-health-care-services/certified-providers/rest-homes)). License: [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/).

Changes made: columns renamed, dates reformatted, old district health boards mapped to Health New Zealand districts and regions, extra commas removed from text, and map coordinates added. Coordinates come from OpenStreetMap: (c) OpenStreetMap contributors, licensed under the [Open Database License (ODbL)](https://opendatacommons.org/licenses/odbl/). This API is not endorsed by the Ministry of Health or Health New Zealand. Certificates and services change, so check HealthCert before you rely on a row.

[Back to the API list](../../README.md)
