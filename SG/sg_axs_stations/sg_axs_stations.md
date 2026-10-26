# Singapore AXS Payment Stations (Partial Sample)

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 27](https://img.shields.io/badge/records-27-lightgrey)

A small, partial list of 27 AXS self service payment machines in Singapore that volunteers have mapped in OpenStreetMap: machine type, partial address, postal code where known, floor level, opening hours, wheelchair access and map coordinates. It is NOT the full AXS network, which has many more stations. Use it as a sample of locations, not as a complete locator.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/sg_axs_stations` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: none. Optional: 11 fields. Disallowed: 3 fields.

Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `__id` | Optional | No | Yes | Row ID. Range supported, for example `__id=3..10`. |
| `osm_id` | Optional | No | No | OpenStreetMap node id in the form node/123456; exact match only. |
| `name` | Optional | Yes | No | Name tag in OpenStreetMap, AXS or AXS Machine. |
| `machine_type` | Optional | No | No | Payment kiosk (25 rows) or ATM (2 rows), from the OpenStreetMap amenity tag. |
| `address[block]` | Optional | Yes | No | Block or house number where OpenStreetMap has it; blank for most rows (22 of 27). |
| `address[street]` | Optional | Yes | No | Street name where OpenStreetMap has it; blank for most rows (22 of 27). |
| `address[postalcode]` | Optional | No | No | Six digit postal code as text; filled for 1 row only, because the rest are not in the source. |
| `osm_level` | Optional | No | No | Raw OpenStreetMap level tag as text; 0 is ground floor, -1 is one floor below, 1 is one floor up; blank for many rows. |
| `location[latitude]` | Optional | No | Yes | Latitude in decimal degrees (WGS84) of the mapped machine; use a range to search a box. |
| `location[longitude]` | Optional | No | Yes | Longitude in decimal degrees (WGS84) of the mapped machine; use a range to search a box. |
| `last_checked` | Optional | No | No | Date a mapper last checked the entry in OpenStreetMap (YYYY-MM-DD); blank for many rows. |
| `address[level]` | Disallowed | No | No | Floor or unit text such as #01 or #B1 where OpenStreetMap has addr:floor; blank for most rows. |
| `opening_hours` | Disallowed | No | No | Opening hours text from OpenStreetMap, such as 24/7; blank for most rows. |
| `wheelchair_access` | Disallowed | No | No | OpenStreetMap wheelchair tag, yes where tagged; blank when unknown. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

Bracket parameters such as `location[latitude]` need `curl -g` (or `%5B` and `%5D`) so that curl does not read the brackets as a glob.

## Response fields

Nested objects are shown with dots, for example `address.street` is the `street` key inside the `address` object.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Row ID. |
| `osm_id` | string | OpenStreetMap node id in the form node/123456; exact match only. |
| `name` | string | Name tag in OpenStreetMap, AXS or AXS Machine. |
| `machine_type` | string | Payment kiosk (25 rows) or ATM (2 rows), from the OpenStreetMap amenity tag. |
| `address.block` | string | Block or house number where OpenStreetMap has it; blank for most rows (22 of 27). |
| `address.street` | string | Street name where OpenStreetMap has it; blank for most rows (22 of 27). |
| `address.level` | string | Floor or unit text such as #01 or #B1 where OpenStreetMap has addr:floor; blank for most rows. |
| `address.postalcode` | string | Six digit postal code as text; filled for 1 row only, because the rest are not in the source. |
| `osm_level` | string | Raw OpenStreetMap level tag as text; 0 is ground floor, -1 is one floor below, 1 is one floor up; blank for many rows. |
| `opening_hours` | string | Opening hours text from OpenStreetMap, such as 24/7; blank for most rows. |
| `wheelchair_access` | string | OpenStreetMap wheelchair tag, yes where tagged; blank when unknown. |
| `location.latitude` | number | Latitude in decimal degrees (WGS84) of the mapped machine. |
| `location.longitude` | number | Longitude in decimal degrees (WGS84) of the mapped machine. |
| `last_checked` | string | Date a mapper last checked the entry in OpenStreetMap (YYYY-MM-DD); blank for many rows. |

## Example

The two machines mapped as ATM type.

```bash
curl -s "https://api.jael.ee/JLEE/sg_axs_stations?machine_type=ATM"
```

Response (2 rows):

```json
[
  {
    "__id": 1,
    "osm_id": "node/5865405050",
    "name": "AXS",
    "machine_type": "ATM",
    "address": {
      "block": null,
      "street": null,
      "level": null,
      "postalcode": null
    },
    "osm_level": null,
    "opening_hours": null,
    "wheelchair_access": null,
    "location": {
      "latitude": 1.332224,
      "longitude": 103.9136874
    },
    "last_checked": null
  },
  {
    "__id": 10,
    "osm_id": "node/7617959145",
    "name": "AXS Machine",
    "machine_type": "ATM",
    "address": {
      "block": null,
      "street": null,
      "level": null,
      "postalcode": null
    },
    "osm_level": null,
    "opening_hours": null,
    "wheelchair_access": null,
    "location": {
      "latitude": 1.4427807,
      "longitude": 103.7911257
    },
    "last_checked": null
  }
]
```

Another call:

Payment kiosks on any street whose name contains Road.

```bash
curl -sg "https://api.jael.ee/JLEE/sg_axs_stations?machine_type=Payment%20kiosk&address[street]=*Road*"
```

Returns 3 rows.

## Data source

AXS payment terminals and ATMs mapped in OpenStreetMap, so this is a partial community sample and not the full AXS network. The official AXS locator was not used. Data © OpenStreetMap contributors, available under the [Open Database License](https://www.openstreetmap.org/copyright).

[Back to all APIs](../../README.md)
