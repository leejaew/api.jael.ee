# Singapore Civil Defence Public Shelters

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 589](https://img.shields.io/badge/records-589-lightgrey)

Civil Defence public shelters in Singapore from SCDF, 589 rows, with host site name, host type (HDB, MRT Station, School and others), address parts, postal code and coordinates. Coordinates were found by postal code lookup, since the source has none. Filter by type, name, street, postal code or a latitude and longitude range.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/sg_cd_public_shelters` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: none. Optional: 10 fields. Disallowed: none.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `__id` | Optional | No | Yes | Row ID. Range supported, for example `__id=3..10`. |
| `name` | Optional | Yes | No | Optional filter for the host site name, such as Telok Blangah CC; 144 distinct names; wildcards work. |
| `type` | Optional | No | No | Optional exact filter for the host kind: HDB, MRT Station, School, Community Club/Centre or Public Development. |
| `address[block]` | Optional | Yes | No | Optional filter for the block or house number, such as 80 or 259C; text, so wildcards work. |
| `address[street]` | Optional | Yes | No | Optional filter for the street name, such as Punggol Field; wildcards work. |
| `address[level]` | Optional | No | No | Optional exact filter for the floor, such as #01 or #B1; blank for 140 rows. |
| `address[unit]` | Optional | No | No | Optional exact filter for the unit number, such as 47; blank when the source has none. |
| `address[postalcode]` | Optional | No | No | Exact six digit postal code, kept as text so leading zeros stay. |
| `location[latitude]` | Optional | No | Yes | Optional filter for latitude in decimal degrees (WGS84); use a range to search a box. Values run from 1.2654 to 1.4465, blank for 2 rows. Found by postal code lookup, not in the source. |
| `location[longitude]` | Optional | No | Yes | Optional filter for longitude in decimal degrees (WGS84); use a range to search a box. Values run from 103.6917 to 103.9641, blank for 2 rows. Found by postal code lookup, not in the source. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

Bracket parameters such as `location[latitude]` need `curl -g` (or `%5B` and `%5D`) so that curl does not read the brackets as a glob.

## Response fields

Nested objects are shown with dots, for example `address.street` is the `street` key inside the `address` object.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Row ID. |
| `name` | string | The host site name, such as Telok Blangah CC; 144 distinct names. |
| `type` | string | The host kind: HDB, MRT Station, School, Community Club/Centre or Public Development. |
| `address.block` | string | The block or house number, such as 80 or 259C; text, so wildcards work. |
| `address.street` | string | The street name, such as Punggol Field. |
| `address.level` | string | The floor, such as #01 or #B1; blank for 140 rows. |
| `address.unit` | string | The unit number, such as 47; blank when the source has none. |
| `address.postalcode` | string | Six digit postal code, kept as text so leading zeros stay. |
| `location.latitude` | number | Latitude in decimal degrees (WGS84). |
| `location.longitude` | number | Longitude in decimal degrees (WGS84). |

## Example

Shelters in MRT stations.

```bash
curl -s "https://api.jael.ee/JLEE/sg_cd_public_shelters?type=MRT%20Station"
```

Response (59 rows, first 2 shown):

```json
[
  {
    "__id": 466,
    "name": "Labrador Park MRT Station (Circle Line)",
    "type": "MRT Station",
    "address": {
      "block": "701",
      "street": "Telok Blangah Road",
      "level": null,
      "unit": null,
      "postalcode": "109029"
    },
    "location": {
      "latitude": 1.272254,
      "longitude": 103.802632
    }
  },
  {
    "__id": 467,
    "name": "Haw Par Villa MRT Station (Circle Line)",
    "type": "MRT Station",
    "address": {
      "block": "270",
      "street": "Pasir Panjang Road",
      "level": null,
      "unit": null,
      "postalcode": "117396"
    },
    "location": {
      "latitude": 1.282571,
      "longitude": 103.781753
    }
  }
]
```

Another call:

Shelters in schools on a Road.

```bash
curl -sg "https://api.jael.ee/JLEE/sg_cd_public_shelters?type=School&address[street]=*Road*"
```

Returns 15 rows.

## Data source

Civil Defence Public Shelters, Singapore Civil Defence Force, data.gov.sg (https://data.gov.sg), dataset d_291795a678b8cf82f108780a6235ce18, Singapore Open Data Licence version 1.0 (https://data.gov.sg/open-data-licence). Source last updated December 3, 2024. The source has no coordinates, so latitude and longitude were found by postal code lookup with OneMap (Singapore Land Authority); 2 rows have no match. Host type, block, street, floor and unit are read from the source description.

[Back to all APIs](../../README.md)
