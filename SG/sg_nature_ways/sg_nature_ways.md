# Singapore Nature Ways

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 56](https://img.shields.io/badge/records-56-lightgrey)

The 56 Nature Ways in Singapore, tree lined roadside routes planted by NParks, one row per route, with name, length in meters and the route midpoint. Filter by name or a range on length_m or the coordinates.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/sg_nature_ways` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: none. Optional: 5 fields. Disallowed: none.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `__id` | Optional | No | No | Row ID. |
| `name` | Optional | Yes | No | Optional filter for the nature way name, such as Punggol Nature Way; wildcards work. |
| `length_m` | Optional | No | Yes | Optional filter for the total length in meters, computed from the line shape; from 582.9 to 20,763.1. |
| `location[latitude]` | Optional | No | Yes | Optional filter for latitude in decimal degrees (WGS84); use a range to search a box. Values run from 1.271 to 1.4586. |
| `location[longitude]` | Optional | No | Yes | Optional filter for longitude in decimal degrees (WGS84); use a range to search a box. Values run from 103.7053 to 103.9512. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

Bracket parameters such as `location[latitude]` need `curl -g` (or `%5B` and `%5D`) so that curl does not read the brackets as a glob.

## Response fields

Nested objects are shown with dots, for example `address.street` is the `street` key inside the `address` object.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Row ID. |
| `name` | string | The nature way name, such as Punggol Nature Way. |
| `length_m` | number | The total length in meters, computed from the line shape; from 582.9 to 20,763.1. |
| `location.latitude` | number | Latitude in decimal degrees (WGS84). |
| `location.longitude` | number | Longitude in decimal degrees (WGS84). |

## Example

Nature ways in Tampines.

```bash
curl -s "https://api.jael.ee/JLEE/sg_nature_ways?name=*Tampines*"
```

Response (3 rows, first 2 shown):

```json
[
  {
    "__id": 3,
    "name": "Tampines Nature Way",
    "length_m": 20763.1,
    "location": {
      "latitude": 1.370865,
      "longitude": 103.938849
    }
  },
  {
    "__id": 12,
    "name": "Tampines East Nature Way",
    "length_m": 2556.5,
    "location": {
      "latitude": 1.348922,
      "longitude": 103.94822
    }
  }
]
```

Another call:

Routes with Nature Way in the name.

```bash
curl -sg "https://api.jael.ee/JLEE/sg_nature_ways?name=*Nature%20Way*"
```

Returns 56 rows.

## Data source

Nature Ways, National Parks Board, data.gov.sg (https://data.gov.sg), dataset d_af948e3f29cd12bc8b0caea19ae68286, Singapore Open Data Licence version 1.0 (https://data.gov.sg/open-data-licence). Source last updated January 24, 2025. Length in meters is computed from each route shape; latitude and longitude are the midpoint along the route.

[Back to all APIs](../../README.md)
