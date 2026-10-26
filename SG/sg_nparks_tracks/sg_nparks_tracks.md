# Singapore NParks Tracks

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 15425](https://img.shields.io/badge/records-15425-lightgrey)

Paths in Singapore parks and park connectors from NParks, one row per line, with park, path type, width band, user category, whether walking, cycling, wheeling and PMDs are allowed, length in meters and the path midpoint. Filter by park, type, ama_category, the allow flags (Y or N) or a range on length_m.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/sg_nparks_tracks` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: none. Optional: 12 fields. Disallowed: none.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `park` | Optional | Yes | No | Optional filter for the park or connector name, such as PUNGGOL WATERWAY PK or Telok Blangah Hill Park; blank for 279 rows; wildcards work. |
| `type` | Optional | No | No | Optional exact filter for the path kind: Footpath, Staircase, Cycle Track, Service Track/Road, Boardwalk/Trail, Bridge/Drain Crossing, Pedestrian Overhead Bridge or Underpass. |
| `park_type` | Optional | No | No | Optional exact filter: Park or Park Connector. |
| `path_width` | Optional | No | No | Optional exact filter for the width band as text: <2.5 or >2.5 but <4 (meters). |
| `ama_category` | Optional | No | No | Optional exact filter for who may use the path, such as Walkers + Cyclists or Cyclists Only; blank for 23 rows. |
| `allow_walking` | Optional | No | No | Optional exact filter, Y or N: walking allowed; blank for 23 rows. |
| `allow_cycling` | Optional | No | No | Optional exact filter, Y or N: cycling allowed; blank for 23 rows. |
| `allow_wheeling` | Optional | No | No | Optional exact filter, Y or N: wheelchairs and prams allowed; blank for 23 rows. |
| `allow_pmd` | Optional | No | No | Optional exact filter, Y or N: personal mobility devices allowed; blank for 23 rows. |
| `length_m` | Optional | No | Yes | Optional filter for the path length in meters, computed from the line shape; from 0 to about 13,433. |
| `location[latitude]` | Optional | No | Yes | Optional filter for latitude in decimal degrees (WGS84); use a range to search a box. Values run from 1.2642 to 1.4639. |
| `location[longitude]` | Optional | No | Yes | Optional filter for longitude in decimal degrees (WGS84); use a range to search a box. Values run from 103.6927 to 104.0217. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

Bracket parameters such as `location[latitude]` need `curl -g` (or `%5B` and `%5D`) so that curl does not read the brackets as a glob.

## Response fields

Nested objects are shown with dots, for example `address.street` is the `street` key inside the `address` object.

| Field | Type | Description |
|---|---|---|
| `park` | string | The park or connector name, such as PUNGGOL WATERWAY PK or Telok Blangah Hill Park; blank for 279 rows. |
| `type` | string | The path kind: Footpath, Staircase, Cycle Track, Service Track/Road, Boardwalk/Trail, Bridge/Drain Crossing, Pedestrian Overhead Bridge or Underpass. |
| `park_type` | string | Optional exact filter: Park or Park Connector. |
| `path_width` | string | The width band as text: <2.5 or >2.5 but <4 (meters). |
| `ama_category` | string | Who may use the path, such as Walkers + Cyclists or Cyclists Only; blank for 23 rows. |
| `allow_walking` | string | Optional exact filter, Y or N: walking allowed; blank for 23 rows. |
| `allow_cycling` | string | Optional exact filter, Y or N: cycling allowed; blank for 23 rows. |
| `allow_wheeling` | string | Optional exact filter, Y or N: wheelchairs and prams allowed; blank for 23 rows. |
| `allow_pmd` | string | Optional exact filter, Y or N: personal mobility devices allowed; blank for 23 rows. |
| `length_m` | number | The path length in meters, computed from the line shape; from 0 to about 13,433. |
| `location.latitude` | number | Latitude in decimal degrees (WGS84). |
| `location.longitude` | number | Longitude in decimal degrees (WGS84). |

## Example

Cycle tracks that allow personal mobility devices.

```bash
curl -s "https://api.jael.ee/JLEE/sg_nparks_tracks?type=Cycle%20Track&allow_pmd=Y"
```

Response (950 rows, first 2 shown):

```json
[
  {
    "park": "PUB - WH/3 WEST COAST HIGHWAY",
    "type": "Cycle Track",
    "park_type": "Park",
    "path_width": "<2.5",
    "ama_category": "Walkers + Cyclists + PMDs",
    "allow_walking": "Y",
    "allow_cycling": "Y",
    "allow_wheeling": "Y",
    "allow_pmd": "Y",
    "length_m": 1604.7,
    "location": {
      "latitude": 1.288948,
      "longitude": 103.770348
    }
  },
  {
    "park": "PASIR RIS PK",
    "type": "Cycle Track",
    "park_type": "Park",
    "path_width": "<2.5",
    "ama_category": "Walkers + Cyclists + PMDs",
    "allow_walking": "Y",
    "allow_cycling": "Y",
    "allow_wheeling": "Y",
    "allow_pmd": "Y",
    "length_m": 32.3,
    "location": {
      "latitude": 1.380973,
      "longitude": 103.953335
    }
  }
]
```

Another call:

Boardwalks and trails in East Coast Park.

```bash
curl -sg "https://api.jael.ee/JLEE/sg_nparks_tracks?park=*East%20Coast*&type=Boardwalk/Trail"
```

Returns 12 rows.

## Data source

Park and Park Connector Paths, National Parks Board, data.gov.sg (https://data.gov.sg), dataset d_306cc1018cb733346681883ee6d73054, Singapore Open Data Licence version 1.0 (https://data.gov.sg/open-data-licence). Source last updated August 27, 2025. Allow flags stay as Y or N. Length in meters is computed from each line shape; latitude and longitude are the halfway point along the path.

[Back to all APIs](../../README.md)
