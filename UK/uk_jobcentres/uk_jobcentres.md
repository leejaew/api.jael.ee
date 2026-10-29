# UK Jobcentres

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 633](https://img.shields.io/badge/records-633-lightgrey) ![updated 2026-10-29](https://img.shields.io/badge/updated-2026--10--29-orange) ![license OGL v3.0](https://img.shields.io/badge/license-OGL%20v3.0-green)

Every active Jobcentre Plus office in Great Britain, 633 offices from the Department for Work and Pensions (DWP) list of October 2025. Each row has the DWP site id, office name, address, town, postcode, council area, DWP district, group and area, nation and map coordinates. Coordinates are postcode centres, not building points.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/uk_jobcentres` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field (`nation`). Optional: 13 fields. Disallowed: 3 fields.

`nation` must be in every call. Use `nation=*` to list all 633 offices. Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `nation` | Required | Yes | No | `England` (489), `Scotland` (84) or `Wales` (60). Wildcards work, so `nation=*` lists all 633 offices. |
| `site_id` | Optional | No | No | DWP site id, a 6 digit code such as `620335`. Unique per office. |
| `nomis_code` | Optional | No | No | NOMIS office code as text with leading zeros, such as `01773`. Blank for 6 offices. |
| `office_name` | Optional | Yes | No | Office name as DWP lists it, such as `*Torus House*` or `Leeds*`. |
| `address` | Optional | Yes | No | Street address text, such as `*High Street*`. |
| `town` | Optional | Yes | No | Post town, such as `Glasgow` or `*bury`. 531 distinct values. |
| `postcode` | Optional | Yes | No | UK postcode, such as `OX14 3QS` or `OX14*`. |
| `local_authority` | Optional | Yes | No | Council area, such as `Leeds` or `*Glasgow*`. 325 distinct values. |
| `district` | Optional | Yes | No | DWP jobcentre district, such as `Greater Manchester` or `*Wales*`. 35 distinct values. |
| `dwp_group` | Optional | Yes | No | DWP group, 12 values, such as `London` (49) or `South West England` (74). |
| `area` | Optional | Yes | No | DWP area, 5 values, such as `Southern England` (120) or `London & Eastern England` (93). Encode `&` as `%26` in a URL. |
| `latitude` | Optional | No | Yes | Latitude in decimal degrees (WGS 84) of the postcode centre, from 50.10053 to 60.157057. Example: `latitude=51.4..51.6`. |
| `longitude` | Optional | No | Yes | Longitude in decimal degrees (WGS 84) of the postcode centre, from -6.39069 to 1.749493. Example: `longitude=-0.3..0.1`. |
| `geocode_quality` | Optional | No | No | `postcode_centroid` (630) or `manual_geocode` (3 offices with retired postcodes, placed by hand). |
| `notes` | Disallowed | No | No | Our notes on fixed addresses or retired postcodes. Blank for most offices. |
| `source` | Disallowed | No | No | Source and credit for the row. |
| `source_as_of` | Disallowed | No | No | Date of the DWP FOI reply, `2025-10-23` on every row. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

## Response fields

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Unique record ID. |
| `site_id` | string | DWP site id. |
| `nomis_code` | string | NOMIS office code with leading zeros. Null for 6 offices. |
| `office_name` | string | Office name as DWP lists it. |
| `address` | string | Street address, without the postcode. |
| `town` | string | Post town. |
| `postcode` | string | UK postcode. |
| `local_authority` | string | Council area, from the postcode. |
| `district` | string | DWP jobcentre district. |
| `dwp_group` | string | DWP group. |
| `area` | string | DWP area. |
| `nation` | string | `England`, `Scotland` or `Wales`. |
| `latitude` | number | Latitude of the postcode centre (WGS 84), 6 decimals. |
| `longitude` | number | Longitude of the postcode centre (WGS 84), 6 decimals. |
| `geocode_quality` | string | `postcode_centroid` or `manual_geocode`. |
| `notes` | string | Our notes for the row. Null for most offices. |
| `source` | string | Source and credit for the row. |
| `source_as_of` | string | Date of the DWP FOI reply. |

Notes: the list is DWP's "Established Job Centres" annex to FOI reply FOI2025/98661 (23 October 2025), which leaves out temporary sites. Three addresses that did not match their postcode were fixed, and six offices have retired postcodes, so their council area comes from the nearest live postcode. Three of those were placed on the map by hand. Every fix is noted in `notes`. The list was not checked against DWP's own office finder.

## Examples

Jobcentres in Cardiff. This call returns 2 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/uk_jobcentres" --data-urlencode "nation=Wales" --data-urlencode "town=Cardiff"
```

Response excerpt (first of 2 rows):

```json
[
  {
    "__id": 1222,
    "site_id": "620257",
    "nomis_code": "09504",
    "office_name": "Cardiff DWP 377 Cowbridge Road East",
    "address": "Cardiff Alexandra House, 377, Cowbridge Road, East Canton, CARDIFF, South Glamorgan",
    "town": "Cardiff",
    "postcode": "CF5 1WU",
    "local_authority": "Cardiff",
    "district": "South East Wales",
    "dwp_group": "Wales And The Marches",
    "area": "North West England, Wales & The Marches",
    "nation": "Wales",
    "latitude": 51.482013,
    "longitude": -3.208469,
    "geocode_quality": "postcode_centroid",
    "notes": null,
    "source": "DWP FOI2025/98661 via Datadaptive Jobcentre Locations GB v2.0 (https://www.datadaptive.com/?pg=18)",
    "source_as_of": "2025-10-23"
  }
]
```

A wildcard on the postcode. Jobcentres in Glasgow (G) postcodes. This call returns 17 rows, and the first three are Cambuslang DWP Cambuslang Gate, Glasgow DWP Radnor House and Glasgow DWP 3 South Muirhead Road:

```bash
curl -sG "https://api.jael.ee/JLEE/uk_jobcentres" --data-urlencode "nation=Scotland" --data-urlencode "postcode=G*" | jq -r '.[].office_name'
```

A box on latitude and longitude in central London. This call returns 12 rows:

```bash
curl -sG "https://api.jael.ee/JLEE/uk_jobcentres" --data-urlencode "nation=England" --data-urlencode "latitude=51.45..51.55" --data-urlencode "longitude=-0.2..0.0" | jq -r '.[].office_name'
```

A wildcard on the office name across all nations. This call returns 1 row, Abingdon DWP Torus House:

```bash
curl -sG "https://api.jael.ee/JLEE/uk_jobcentres" --data-urlencode "nation=*" --data-urlencode "office_name=*Torus*" | jq -r '.[].office_name'
```

## Data source

The list comes from the Department for Work and Pensions reply to Freedom of Information request FOI2025/98661 (23 October 2025), as published in the [Datadaptive Jobcentre Locations GB](https://www.datadaptive.com/?pg=18) dataset (version 2.0, 4 November 2025). It is released under the [Open Government Licence v3.0](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/), and DWP confirmed there are no limits on reuse of the reply. Postcode coordinates come from the ONS postcode file (August 2025), and council areas come from [postcodes.io](https://postcodes.io). DWP does not endorse this API.

Credit lines:

* Contains public sector information licensed under the Open Government Licence v3.0.
* Contains OS data © Crown copyright and database right 2025.
* Contains Royal Mail data © Royal Mail copyright and database right 2025.
* Source: Office for National Statistics licensed under the Open Government Licence v3.0.

[Back to the API list](../../README.md)
