# Singapore MRT Station Exits

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 597](https://img.shields.io/badge/records-597-lightgrey)

Entrance and exit points of Singapore MRT stations. Each row is one exit, in a GeoJSON feature layout: a point geometry with longitude and latitude, and properties for the station name and exit code. A station has one row for every exit.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/sg_lta_mrt_station_exit_geojson_api` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: none. Optional: 2 fields. Disallowed: 7 fields.

Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `properties[STATION_NA]` | Optional | Yes | No | Station name in capital letters, for example `*ORCHARD*`. |
| `__id` | Optional | No | Yes | Row ID. Range supported, for example `__id=3..10`. |
| `feature` | Disallowed | No | No | GeoJSON object type. Always `Feature`. |
| `geometry[type]` | Disallowed | No | No | GeoJSON geometry type. Always `Point`. |
| `geometry[coordinates]` | Disallowed | No | No | Longitude and latitude of the exit, as one text value in the form `"longitude, latitude"`. |
| `properties[OBJECT_ID]` | Disallowed | No | No | Object ID from the source layer. |
| `properties[EXIT_CODE]` | Disallowed | No | No | Exit label, for example `Exit 1` or `Exit A`. |
| `properties[INC_CRC]` | Disallowed | No | No | Checksum code of the source record. |
| `properties[FMEL_UPD_D]` | Disallowed | No | No | Date the source record was updated, in `YYYYMMDDhhmmss` form with a decimal part. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

Bracket parameters such as `address[postalcode]` need `curl -g` (or `%5B` and `%5D`) so that curl does not read the brackets as a glob.

## Response fields

Nested objects are shown with dots, for example `address.street` is the `street` key inside the `address` object.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Row ID, from 1 to 597. |
| `feature` | string | GeoJSON object type. Always `Feature`. |
| `geometry.type` | string | GeoJSON geometry type. Always `Point`. |
| `geometry.coordinates` | string | Longitude and latitude of the exit, as one text value in the form `"longitude, latitude"`. |
| `properties.OBJECT_ID` | integer | Object ID from the source layer. |
| `properties.STATION_NA` | string | Station name in capital letters, for example `BRIGHT HILL MRT STATION`. |
| `properties.EXIT_CODE` | string | Exit label, for example `Exit 1` or `Exit A`. |
| `properties.INC_CRC` | string | Checksum code of the source record. |
| `properties.FMEL_UPD_D` | number | Date the source record was updated, in `YYYYMMDDhhmmss` form with a decimal part. |

## Example

All exits of stations with BRIGHT in the name.

```bash
curl -s -g "https://api.jael.ee/JLEE/sg_lta_mrt_station_exit_geojson_api?properties[STATION_NA]=*BRIGHT*"
```

Response, showing 2 of 4 rows:

```json
[
  {
    "__id": 1,
    "feature": "Feature",
    "geometry": {
      "type": "Point",
      "coordinates": "103.83363541399386, 1.3640371821656152"
    },
    "properties": {
      "OBJECT_ID": 17885,
      "STATION_NA": "BRIGHT HILL MRT STATION",
      "EXIT_CODE": "Exit 1",
      "INC_CRC": "CFB9350F44E8991A",
      "FMEL_UPD_D": 20251200000000.0
    }
  },
  {
    "__id": 2,
    "feature": "Feature",
    "geometry": {
      "type": "Point",
      "coordinates": "103.83351063156859, 1.3632827623551678"
    },
    "properties": {
      "OBJECT_ID": 17886,
      "STATION_NA": "BRIGHT HILL MRT STATION",
      "EXIT_CODE": "Exit 2",
      "INC_CRC": "F273E9D550B06062",
      "FMEL_UPD_D": 20251200000000.0
    }
  },
  ...
]
```

## Data source

Land Transport Authority (LTA) MRT station exit layer, Singapore.

[Back to all APIs](../../README.md)
