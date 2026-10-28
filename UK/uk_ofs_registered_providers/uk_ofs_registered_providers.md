# UK Office for Students Registered Providers

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 425](https://img.shields.io/badge/records-425-lightgrey) ![updated 2026-10-28](https://img.shields.io/badge/updated-2026--10--28-orange)

Every higher education provider on the Office for Students Register in England, 425 in all. Each row gives the legal and trading names, UKPRN, registration category, degree awarding powers, university title, TEF status, the 2026 to 27 fee limit, address, region and the map position of the postcode.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/uk_ofs_registered_providers` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`region`). Optional: 15 fields. Disallowed: 5 fields.

`region` must be in every call. Use `region=*` to list all 425 providers. Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `region` | Required | Yes | No | English region of the provider: `London` (145), `South East` (56), `North West` (51), `South West` (38), `Yorkshire and The Humber` (35), `West Midlands` (35), `East of England` (28), `East Midlands` (23) or `North East` (14). Wildcards work, so `region=*` lists all 425 providers. |
| `name` | Optional | Yes | No | Provider legal name, such as `*Imperial*` or `University of*`. |
| `trading_name` | Optional | Yes | No | Other names the provider trades under, separated by semicolons. Blank for 215 providers. |
| `ukprn` | Optional | No | No | UK Provider Reference Number, 8 digits as text, such as `10007795`. |
| `registration_category` | Optional | No | No | `Approved (fee cap)` (342 providers) or `Approved` (83). |
| `degree_awarding_powers` | Optional | No | No | `Yes` (169) or `No` (256), for providers that hold their own degree awarding powers. |
| `degree_awarding_level` | Optional | No | No | Highest level of degree awarding powers: `None`, `Research`, `Taught`, `Taught (new powers)`, `Bachelor's`, `Bachelor's (new powers)` or `Foundation`. |
| `university_title` | Optional | No | No | `Yes` (122) or `No` (303), for the right to use the university title. |
| `tef_status` | Optional | No | No | `Participated in TEF 2023` (219), `Did not participate in TEF 2023` (203) or `Not applicable` (3). |
| `tef_award` | Optional | No | No | `Yes` or `No`, for whether the provider held a valid teaching excellence award when the 2026 to 27 fee limits were set. Blank for 86 providers. |
| `fee_limit_type` | Optional | No | No | Fee limit group: `Higher` (259), `Basic` (73), `No fee limit` (71) or `Not applicable` (22). |
| `fee_limit` | Optional | No | Yes | Full time undergraduate fee limit in GBP a year for courses starting in 2026 to 27: 6350, 6525, 9525 or 9790. Blank where no fee cap applies. Use a range such as `fee_limit=9000..10000`. |
| `address` | Optional | Yes | No | Address on one line, without the postcode. |
| `city` | Optional | Yes | No | Town or city, such as `London`, `Leeds` or `*ford*`. |
| `postcode` | Optional | Yes | No | UK postcode as text, such as `SW7 2AZ` or `SW7*`. |
| `latitude` | Optional | No | Yes | Latitude of the postcode centre in decimal degrees (WGS 84), from 50.149612 to 54.986238. Use a range to search a box, for example `latitude=51.4..51.6`. |
| `longitude` | Optional | No | Yes | Longitude of the postcode centre in decimal degrees (WGS 84), from -5.10083 to 1.435053. Use a range to search a box, for example `longitude=-0.3..0.1`. |
| `registration_status` | Disallowed | No | No | Registration status on the Register. It is `Registered` for every row. |
| `fee_limit_note` | Disallowed | No | No | Short note where the fee amount and the current Register group differ, or no amount is published. |
| `website` | Disallowed | No | No | Provider website address as published in the Register. |
| `source` | Disallowed | No | No | Data source and licence credit for the row. |
| `updated` | Disallowed | No | No | Date the row was last checked, such as `2026-10-28`. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `name` | string | Provider legal name. |
| `trading_name` | string | Other trading names, separated by semicolons. |
| `ukprn` | string | UK Provider Reference Number. |
| `registration_status` | string | `Registered` for every row. |
| `registration_category` | string | `Approved (fee cap)` or `Approved`. |
| `degree_awarding_powers` | string | `Yes` or `No`. |
| `degree_awarding_level` | string | Highest level of degree awarding powers, or `None`. |
| `university_title` | string | `Yes` or `No`. |
| `tef_status` | string | Whether the provider took part in TEF 2023. |
| `tef_award` | string | `Yes` or `No`, blank where no fee cap applies. |
| `fee_limit_type` | string | Fee limit group on the Register. |
| `fee_limit` | integer | Full time undergraduate fee limit in GBP a year for 2026 to 27. |
| `fee_limit_note` | string | Note on 15 rows, otherwise null. |
| `address` | string | Address on one line, without postcode. |
| `city` | string | Town or city. |
| `postcode` | string | UK postcode. |
| `region` | string | English region. |
| `website` | string | Provider website. |
| `latitude` | number | Latitude of the postcode centre in decimal degrees (WGS 84). |
| `longitude` | number | Longitude of the postcode centre in decimal degrees (WGS 84). |
| `source` | string | Where the row came from, with the credit line. |
| `updated` | string | Date the row was last checked. |

Notes: the map position is the centre of the postcode, not the building. Four providers have a terminated postcode on the Register, so their address is out of date: The London School of Economics and Political Science, The Metanoia Institute, PHBS-UK and The Institute for Optimum Nutrition. The teaching rating (Gold, Silver or Bronze) is not included because it has no official download. `tef_status` and `tef_award` are provided instead. `fee_limit_note` marks 12 rows where the fee limit file from 30 March 2026 lists a different group from the current Register. For those rows, `fee_limit` follows the fee limit file.

## Examples

Universities in London, listing the first five. This call returns 24 rows:

```bash
curl -s "https://api.jael.ee/JLEE/uk_ofs_registered_providers?region=London&university_title=Yes" | jq -r '.[0:5][] | "\(.name) | \(.city)"'
```

```text
Imperial College of Science, Technology and Medicine | London
University College London | London
City St George's, University of London | London
Brunel University London | Uxbridge
Middlesex University Higher Education Corporation | London
```

Imperial College, with a wildcard on `name`:

```bash
curl -s "https://api.jael.ee/JLEE/uk_ofs_registered_providers?region=London&name=*Imperial*"
```

Response (1 row):

```json
[
  {
    "__id": 24,
    "name": "Imperial College of Science, Technology and Medicine",
    "trading_name": "Imperial College London",
    "ukprn": "10003270",
    "registration_status": "Registered",
    "registration_category": "Approved (fee cap)",
    "degree_awarding_powers": "Yes",
    "degree_awarding_level": "Research",
    "university_title": "Yes",
    "tef_status": "Participated in TEF 2023",
    "tef_award": "Yes",
    "fee_limit_type": "Higher",
    "fee_limit": 9790,
    "fee_limit_note": null,
    "address": "Exhibition Road, London",
    "city": "London",
    "postcode": "SW7 2AZ",
    "region": "London",
    "website": "https://www.imperial.ac.uk",
    "latitude": 51.499334,
    "longitude": -0.179178,
    "source": "Office for Students Register and fee information 2026-27 (OGL v3.0); locations from postcodes.io (ONS, OS, Royal Mail data)",
    "updated": "2026-10-28"
  }
]
```

Providers in the SW7 postcode area, across all regions, with a wildcard on `postcode`:

```bash
curl -s "https://api.jael.ee/JLEE/uk_ofs_registered_providers?region=*&postcode=SW7*" | jq -r '.[] | "\(.name) | \(.postcode)"'
```

```text
Royal College of Music | SW7 2BS
Imperial College of Science, Technology and Medicine | SW7 2AZ
Institute of Cancer Research: Royal Cancer Hospital (The) | SW7 3RP
Royal College of Art (The) | SW7 2EU
```

Providers in the North East that hold degree awarding powers, with the fee limit. This call returns 8 rows:

```bash
curl -s "https://api.jael.ee/JLEE/uk_ofs_registered_providers?region=North%20East&degree_awarding_powers=Yes" | jq -r '.[] | "\(.name) | \(.fee_limit)"'
```

Providers with a fee limit between 9000 and 10000 GBP, using a range on `fee_limit`. This call returns 259 rows:

```bash
curl -s "https://api.jael.ee/JLEE/uk_ofs_registered_providers?region=*&fee_limit=9000..10000" | jq length
```

Providers in a box around central London, with ranges on latitude and longitude. This call returns 50 rows, and the first three are The Royal Veterinary College, King's College London and University College London:

```bash
curl -s "https://api.jael.ee/JLEE/uk_ofs_registered_providers?region=London&latitude=51.5..51.55&longitude=-0.15..-0.05" | jq -r '.[0:3][].name'
```

## Data source

The provider list, registration category, degree awarding powers, university title, TEF status, fee limit group and address come from the Office for Students Register, checked on 2026-10-28. The fee limit amounts come from the Office for Students fee information for courses starting in 2026 to 27 (limits as of 30 March 2026). Map positions come from postcodes.io, which uses Office for National Statistics, Ordnance Survey and Royal Mail postcode data. The Register lists 425 providers, while the Office for Students guide says 428.

Credit line: Contains information from the Office for Students Register, published by the Office for Students under the [Open Government Licence v3.0](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/). Contains Ordnance Survey data © Crown copyright and database right 2025. Contains Royal Mail data © Royal Mail copyright and database right 2025. Contains National Statistics data © Crown copyright and database right 2025.

[Back to the API list](../../README.md)
