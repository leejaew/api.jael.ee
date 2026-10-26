# Singapore Social Service Offices

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 24](https://img.shields.io/badge/records-24-lightgrey)

The 24 Social Service Offices in Singapore run by the Ministry of Social and Family Development, one row per office, with name, address parts, postal code and coordinates. Filter by name, street, building, postal code or a latitude and longitude range.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/sg_social_service_offices` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: none. Optional: 10 fields. Disallowed: none.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `__id` | Optional | No | No | Row ID. |
| `name` | Optional | Yes | No | Optional filter for the office name, written Social Service Office @ town; wildcards work. |
| `address[block]` | Optional | Yes | No | Optional filter for the block or house number, such as 80 or 259C; text, so wildcards work. |
| `address[street]` | Optional | Yes | No | Optional filter for the street name, such as Punggol Field; wildcards work. |
| `address[level]` | Optional | No | No | Optional exact filter for the floor, such as #01 or #B1; blank when the source has none. |
| `address[unit]` | Optional | No | No | Optional exact filter for the unit number, such as 47; blank when the source has none. |
| `address[building]` | Optional | Yes | No | Optional filter for the building name, such as AMK Hub; blank for 14 of 24 rows; wildcards work. |
| `address[postalcode]` | Optional | No | No | Exact six digit postal code, kept as text so leading zeros stay. |
| `location[latitude]` | Optional | No | Yes | Optional filter for latitude in decimal degrees (WGS84); use a range to search a box. Values run from 1.2812 to 1.4469. |
| `location[longitude]` | Optional | No | Yes | Optional filter for longitude in decimal degrees (WGS84); use a range to search a box. Values run from 103.7165 to 103.9592. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

Bracket parameters such as `location[latitude]` need `curl -g` (or `%5B` and `%5D`) so that curl does not read the brackets as a glob.

## Response fields

Nested objects are shown with dots, for example `address.street` is the `street` key inside the `address` object.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Row ID. |
| `name` | string | The office name, written Social Service Office @ town. |
| `address.block` | string | The block or house number, such as 80 or 259C; text, so wildcards work. |
| `address.street` | string | The street name, such as Punggol Field. |
| `address.level` | string | The floor, such as #01 or #B1; blank when the source has none. |
| `address.unit` | string | The unit number, such as 47; blank when the source has none. |
| `address.building` | string | The building name, such as AMK Hub; blank for 14 of 24 rows. |
| `address.postalcode` | string | Six digit postal code, kept as text so leading zeros stay. |
| `location.latitude` | number | Latitude in decimal degrees (WGS84). |
| `location.longitude` | number | Longitude in decimal degrees (WGS84). |

## Example

The office in Woodlands.

```bash
curl -s "https://api.jael.ee/JLEE/sg_social_service_offices?name=*Woodlands*"
```

Response (1 row):

```json
[
  {
    "__id": 3,
    "name": "Social Service Office @ Woodlands",
    "address": {
      "block": "900",
      "street": "South Woodlands Drive",
      "level": "#03",
      "unit": "01A",
      "building": "Woodlands Civic Centre",
      "postalcode": "730900"
    },
    "location": {
      "latitude": 1.435078,
      "longitude": 103.78662
    }
  }
]
```

Another call:

Offices inside a hub building.

```bash
curl -sg "https://api.jael.ee/JLEE/sg_social_service_offices?address[building]=*Hub*"
```

Returns 3 rows.

## Data source

Social Service Offices, Ministry of Social and Family Development, data.gov.sg (https://data.gov.sg), dataset d_22cfe2aed0bf20a679ab59bcaf0f8248, Singapore Open Data Licence version 1.0 (https://data.gov.sg/open-data-licence). Source last updated September 22, 2026. The source has no phone numbers. The floor comes from the source floor number, for example #01.

[Back to all APIs](../../README.md)
