# Singapore Central Catchment Nature Reserve Amenities

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 449](https://img.shields.io/badge/records-449-lightgrey)

Amenities in the Central Catchment Nature Reserve in Singapore such as huts, toilets, towers, carparks and entrances, one row per point, with name, a numeric type code and coordinates. Filter by name or type_code, or use a latitude and longitude range.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/sg_cnr_amenities` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: none. Optional: 5 fields. Disallowed: none.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `__id` | Optional | No | Yes | Row ID. Range supported, for example `__id=3..10`. |
| `name` | Optional | Yes | No | Optional filter for the amenity name, such as Sunbird Hut or Toilet; blank for 265 of 449 rows; wildcards work. |
| `type_code` | Optional | No | No | Optional exact filter for the source build type code, a whole number from 1 to 12 (no 10); the source gives no legend, but 4 holds toilets and 5 holds carparks in the names seen. |
| `location[latitude]` | Optional | No | Yes | Optional filter for latitude in decimal degrees (WGS84); use a range to search a box. Values run from 1.2624 to 1.4056. |
| `location[longitude]` | Optional | No | Yes | Optional filter for longitude in decimal degrees (WGS84); use a range to search a box. Values run from 103.7533 to 103.8366. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

Bracket parameters such as `location[latitude]` need `curl -g` (or `%5B` and `%5D`) so that curl does not read the brackets as a glob.

## Response fields

Nested objects are shown with dots, for example `address.street` is the `street` key inside the `address` object.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Row ID. |
| `name` | string | The amenity name, such as Sunbird Hut or Toilet; blank for 265 of 449 rows. |
| `type_code` | string | The source build type code, a whole number from 1 to 12 (no 10); the source gives no legend, but 4 holds toilets and 5 holds carparks in the names seen. |
| `location.latitude` | number | Latitude in decimal degrees (WGS84). |
| `location.longitude` | number | Longitude in decimal degrees (WGS84). |

## Example

Amenities with type code 4, mostly toilets.

```bash
curl -s "https://api.jael.ee/JLEE/sg_cnr_amenities?type_code=4"
```

Response (18 rows, first 2 shown):

```json
[
  {
    "__id": 4,
    "name": "Toilet",
    "type_code": "4",
    "location": {
      "latitude": 1.371631,
      "longitude": 103.779744
    }
  },
  {
    "__id": 5,
    "name": "Toilet",
    "type_code": "4",
    "location": {
      "latitude": 1.401138,
      "longitude": 103.817623
    }
  }
]
```

Another call:

Huts by name.

```bash
curl -sg "https://api.jael.ee/JLEE/sg_cnr_amenities?name=*Hut*"
```

Returns 31 rows.

## Data source

Central Catchment Nature Reserve Amenities, National Parks Board, data.gov.sg (https://data.gov.sg), dataset d_19a87bbb86b2c14601a1745076812438, Singapore Open Data Licence version 1.0 (https://data.gov.sg/open-data-licence). Source last updated January 22, 2025. The type_code comes from the source build type, and the source gives no legend for it. The name is blank on 265 of 449 rows.

[Back to all APIs](../../README.md)
