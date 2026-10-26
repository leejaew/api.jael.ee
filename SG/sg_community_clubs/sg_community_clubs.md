# Singapore Community Clubs

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 128](https://img.shields.io/badge/records-128-lightgrey)

Community clubs and PAssion WaVe outlets run by the People's Association in Singapore, one row per outlet, with name, tag, address parts, postal code and map coordinates. Filter by name, tag (cc or passion_wave), street, postal code or a latitude and longitude range.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/sg_community_clubs` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: none. Optional: 10 fields. Disallowed: 1 field.

Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `__id` | Optional | No | Yes | Row ID. Range supported, for example `__id=3..10`. |
| `name` | Optional | Yes | No | Optional filter for the club name, such as Punggol CC; wildcards work. |
| `tag` | Optional | No | No | Optional exact filter for the outlet kind: cc (community club, 122 rows) or passion_wave (6 rows). |
| `address[block]` | Optional | Yes | No | Optional filter for the block or house number, such as 80 or 259C; text, so wildcards work. |
| `address[street]` | Optional | Yes | No | Optional filter for the street name, such as Punggol Field; wildcards work. |
| `address[level]` | Optional | No | No | Optional exact filter for the floor, such as #01 or #B1; blank when the source has none. |
| `address[unit]` | Optional | No | No | Optional exact filter for the unit number, such as 47; blank when the source has none. |
| `address[postalcode]` | Optional | No | No | Exact six digit postal code, kept as text so leading zeros stay. |
| `location[latitude]` | Optional | No | Yes | Optional filter for latitude in decimal degrees (WGS84); use a range to search a box. Values run from 1.2749 to 1.459. |
| `location[longitude]` | Optional | No | Yes | Optional filter for longitude in decimal degrees (WGS84); use a range to search a box. Values run from 103.6923 to 103.9592. |
| `address[building]` | Disallowed | No | No | The building or mall name. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

Bracket parameters such as `location[latitude]` need `curl -g` (or `%5B` and `%5D`) so that curl does not read the brackets as a glob.

## Response fields

Nested objects are shown with dots, for example `address.street` is the `street` key inside the `address` object.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Row ID. |
| `name` | string | The club name, such as Punggol CC. |
| `tag` | string | The outlet kind: cc (community club, 122 rows) or passion_wave (6 rows). |
| `address.block` | string | The block or house number, such as 80 or 259C; text, so wildcards work. |
| `address.street` | string | The street name, such as Punggol Field. |
| `address.level` | string | The floor, such as #01 or #B1; blank when the source has none. |
| `address.unit` | string | The unit number, such as 47; blank when the source has none. |
| `address.building` | string | The building or mall name. |
| `address.postalcode` | string | Six digit postal code, kept as text so leading zeros stay. |
| `location.latitude` | number | Latitude in decimal degrees (WGS84). |
| `location.longitude` | number | Longitude in decimal degrees (WGS84). |

## Example

Community clubs on any street with Avenue in its name.

```bash
curl -sg "https://api.jael.ee/JLEE/sg_community_clubs?tag=cc&address[street]=*Avenue*"
```

Response (23 rows, first 2 shown):

```json
[
  {
    "__id": 2,
    "name": "Punggol CC",
    "tag": "cc",
    "address": {
      "block": "3",
      "street": "Hougang Avenue 6",
      "level": null,
      "unit": null,
      "building": null,
      "postalcode": "538808"
    },
    "location": {
      "latitude": 1.374379,
      "longitude": 103.891815
    }
  },
  {
    "__id": 4,
    "name": "Queenstown CC",
    "tag": "cc",
    "address": {
      "block": "365",
      "street": "Commonwealth Avenue",
      "level": null,
      "unit": null,
      "building": null,
      "postalcode": "149732"
    },
    "location": {
      "latitude": 1.29915,
      "longitude": 103.801305
    }
  }
]
```

Another call:

Outlets whose name starts with Punggol.

```bash
curl -s "https://api.jael.ee/JLEE/sg_community_clubs?name=Punggol*"
```

Returns 5 rows.

## Data source

Community clubs and PAssion WaVe outlets, People's Association, data.gov.sg (https://data.gov.sg), dataset d_9de02d3fb33d96da1855f4fbef549a0f, Singapore Open Data Licence version 1.0 (https://data.gov.sg/open-data-licence). Source last updated September 30, 2026.

[Back to all APIs](../../README.md)
