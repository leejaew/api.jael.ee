# Singapore Park Connector Segments

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 878](https://img.shields.io/badge/records-878-lightgrey)

Segments of the NParks Park Connector Network in Singapore, one row per line segment, with park name, route loop, length in meters, an information link and the segment midpoint. Filter by park, loop, or a range on length_m or the coordinates.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/sg_park_connector_segments` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field. Optional: 5 fields. Disallowed: 1 field.

Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `loop` | Required | Yes | No | Required. Allowed values: Northern Explorer Loop (250); Central Urban Loop (188); Western Adventure Loop (164); Southern Ridges Loop (147); Eastern Coastal Loop (72); North Eastern Riverine Loop (49); Urban Central Loop (8). Use * to list all rows. |
| `__id` | Optional | No | Yes | Row ID. Range supported, for example `__id=3..10`. |
| `park` | Optional | Yes | No | Optional filter for the park or connector name, such as East Coast Park or Whampoa Park Connector; wildcards work. |
| `length_m` | Optional | No | Yes | Optional filter for the segment length in meters, computed from the line shape; from 0.1 to 14,010.2. |
| `location[latitude]` | Optional | No | Yes | Optional filter for latitude in decimal degrees (WGS84); use a range to search a box. Values run from 1.2738 to 1.4613. |
| `location[longitude]` | Optional | No | Yes | Optional filter for longitude in decimal degrees (WGS84); use a range to search a box. Values run from 103.6968 to 104.0227. |
| `more_info` | Disallowed | No | No | Link to NParks route information; returned in results, not used as a filter. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

Bracket parameters such as `location[latitude]` need `curl -g` (or `%5B` and `%5D`) so that curl does not read the brackets as a glob.

## Response fields

Nested objects are shown with dots, for example `address.street` is the `street` key inside the `address` object.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Row ID. |
| `park` | string | The park or connector name, such as East Coast Park or Whampoa Park Connector. |
| `loop` | string | The route loop, such as Northern Explorer Loop or Central Urban Loop (7 loops). |
| `more_info` | string | Link to NParks route information; returned in results, not used as a filter. |
| `length_m` | number | The segment length in meters, computed from the line shape; from 0.1 to 14,010.2. |
| `location.latitude` | number | Latitude in decimal degrees (WGS84). |
| `location.longitude` | number | Longitude in decimal degrees (WGS84). |

## Example

Segments of the Eastern Coastal Loop in East Coast Park.

```bash
curl -s "https://api.jael.ee/JLEE/sg_park_connector_segments?loop=Eastern%20Coastal%20Loop&park=*East%20Coast*"
```

Response (16 rows, first 2 shown):

```json
[
  {
    "__id": 2,
    "park": "East Coast Park (Tanjong Rhu to Area C)",
    "loop": "Eastern Coastal Loop",
    "more_info": "http://www.nparks.gov.sg/pcn",
    "length_m": 2072.8,
    "location": {
      "latitude": 1.293722,
      "longitude": 103.87628
    }
  },
  {
    "__id": 8,
    "park": "East Coast Park",
    "loop": "Eastern Coastal Loop",
    "more_info": "http://www.nparks.gov.sg/pcn",
    "length_m": 481.6,
    "location": {
      "latitude": 1.301898,
      "longitude": 103.916715
    }
  }
]
```

Another call:

Park connector segments on the Western Adventure Loop.

```bash
curl -s "https://api.jael.ee/JLEE/sg_park_connector_segments?park=*Connector*&loop=Western%20Adventure%20Loop"
```

Returns 25 rows.

## Data source

Park Connector Loop, National Parks Board, data.gov.sg (https://data.gov.sg), dataset d_a69ef89737379f231d2ae93fd1c5707f, Singapore Open Data Licence version 1.0 (https://data.gov.sg/open-data-licence). Source last updated May 11, 2026. Length in meters is computed from each line shape; latitude and longitude are the halfway point along the segment.

[Back to all APIs](../../README.md)
