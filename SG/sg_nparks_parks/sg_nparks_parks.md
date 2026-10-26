# Singapore NParks Parks

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 462](https://img.shields.io/badge/records-462-lightgrey)

Parks in Singapore managed by NParks, one row per park as a single map point, with the park name and coordinates. Filter by name with wildcards, for example *PG for playgrounds, or use a latitude and longitude range.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/sg_nparks_parks` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: none. Optional: 4 fields. Disallowed: none.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `__id` | Optional | No | Yes | Row ID. Range supported, for example `__id=3..10`. |
| `name` | Optional | Yes | No | Optional filter for the park name, mostly in capitals with a type code at the end, such as GROVE LANE PG; wildcards work. |
| `location[latitude]` | Optional | No | Yes | Optional filter for latitude in decimal degrees (WGS84); use a range to search a box. Values run from 1.2144 to 1.4619. |
| `location[longitude]` | Optional | No | Yes | Optional filter for longitude in decimal degrees (WGS84); use a range to search a box. Values run from 103.6929 to 104.0538. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

Bracket parameters such as `location[latitude]` need `curl -g` (or `%5B` and `%5D`) so that curl does not read the brackets as a glob.

## Response fields

Nested objects are shown with dots, for example `address.street` is the `street` key inside the `address` object.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Row ID. |
| `name` | string | The park name, mostly in capitals with a type code at the end, such as GROVE LANE PG. |
| `location.latitude` | number | Latitude in decimal degrees (WGS84). |
| `location.longitude` | number | Longitude in decimal degrees (WGS84). |

## Example

Parks with reservoir in the name.

```bash
curl -s "https://api.jael.ee/JLEE/sg_nparks_parks?name=*RESERVOIR*"
```

Response (9 rows, first 2 shown):

```json
[
  {
    "__id": 99,
    "name": "MACRITCHIE RESERVOIR PK",
    "location": {
      "latitude": 1.343451,
      "longitude": 103.836281
    }
  },
  {
    "__id": 105,
    "name": "LOWER SELETAR RESERVOIR PARK",
    "location": {
      "latitude": 1.40744,
      "longitude": 103.828893
    }
  }
]
```

Another call:

Parks whose name starts with BEDOK.

```bash
curl -sg "https://api.jael.ee/JLEE/sg_nparks_parks?name=BEDOK*"
```

Returns 4 rows.

## Data source

Parks, National Parks Board, data.gov.sg (https://data.gov.sg), dataset d_0542d48f0991541706b58059381a6eca, Singapore Open Data Licence version 1.0 (https://data.gov.sg/open-data-licence). Source last updated September 3, 2026. Each park is reduced to one map point; the source projected coordinates are converted to latitude and longitude.

[Back to all APIs](../../README.md)
