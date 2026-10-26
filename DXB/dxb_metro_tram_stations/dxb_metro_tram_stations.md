# Dubai Metro and Tram Stations

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 66](https://img.shields.io/badge/records-66-lightgrey)

Stations on the Dubai Metro Red Line, the Dubai Metro Green Line and the Dubai Tram Line. Each row is one station with its coordinates and a text description of its exits and what each exit connects to.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/dxb_metro_tram_stations` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `station_name` | Optional | Yes | No | Station name, for example `*Mall*`. |
| `line_name` | Optional | Yes | No | `Red Metro Line`, `Green Metro Line` or `Tram Line`. |
| `exits` | Optional | Yes | No | Text search in the exit descriptions, for example `*Dubai Mall*`. |
| `__id` | Optional | No | No | Row ID. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../README.md#how-filters-work) for the wildcard and range syntax.
* Any field that is not in this table is returned in the output but cannot be used as a filter. Sending one returns an error.

Bracket parameters such as `address[postalcode]` need `curl -g` (or `%5B` and `%5D`) so that curl does not read the brackets as a glob.

## Response fields

Nested objects are shown with dots, for example `address.street` is the `street` key inside the `address` object.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Row ID, from 1 to 66. |
| `station_name` | string | Station name, including the line and the words Metro Station or Tram Station. |
| `line_name` | string | Line the station is on: `Red Metro Line`, `Green Metro Line` or `Tram Line`. |
| `station_location.longitude` | number | Longitude in decimal degrees. |
| `station_location.latitude` | number | Latitude in decimal degrees. |
| `loading_date` | integer | Date the row was loaded, as a number in `YYYYMMDD` format. |
| `ingestion_id` | integer | Internal load batch number. |
| `exits` | string | Exit numbers with a short description of where each exit leads, separated by semicolons. |

## Example

All Dubai Tram stations.

```bash
curl -s "https://api.jael.ee/JLEE/dxb_metro_tram_stations?line_name=*Tram*"
```

Response, showing 2 of 11 rows:

```json
[
  {
    "__id": 56,
    "station_name": "Jumeirah Beach Residence 1 Tram Station",
    "line_name": "Tram Line",
    "station_location": {
      "longitude": 55.13824,
      "latitude": 25.07974
    },
    "loading_date": 20260420,
    "ingestion_id": 0,
    "exits": "Located on Al Mamsha Street (King Salman Bin Abdulaziz Al Saud Street) in Jumeirah Beac..."
  },
  {
    "__id": 57,
    "station_name": "Jumeirah Beach Residence 2 Tram Station",
    "line_name": "Tram Line",
    "station_location": {
      "longitude": 55.13226,
      "latitude": 25.07385
    },
    "loading_date": 20260420,
    "ingestion_id": 0,
    "exits": "Provides access to Al Mamsha Street (King Salman Bin Abdulaziz Al Saud Street) in Jumei..."
  },
  ...
]
```

[Back to all APIs](../README.md)
