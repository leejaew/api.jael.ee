# Singapore Park Facilities

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 14754](https://img.shields.io/badge/records-14754-lightgrey)

Facility points in Singapore parks and nature areas from NParks, one row per point such as a carpark, noticeboard, toilet, playground or BBQ pit, with a class, an optional name and coordinates. Filter by class (24 kinds) or name, or use a latitude and longitude range to find facilities near a spot.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/sg_nparks_park_facilities` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field. Optional: 3 fields. Disallowed: none.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `class` | Required | Yes | No | Required. Allowed values: CARPARK (9285); NOTICEBOARD (2619); ACCESS POINT (601); MAPBOARD (528); FITNESS AREA (475); PLAYGROUND (387); BBQ PIT (225); FOOT RELAX (164); TOILET (132); DRINKING FOUNTAIN (113); SHELTER (47); ALLOTMENT GARDEN (34); JETTY (25); BICYCLE PARKING (21); THERAPEUTIC GARDEN (21); MULTIPURPOSE COURT (16); BICYCLE RENTAL SHOP (16); F&B (15); CAMPSITE (8); LOOKOUT POINT (7); AMPHITHEATRE (7); DOG-AREA (3); DROP-OFF/PICK-UP POINT (3); BABY (2). Use * to list all rows. |
| `name` | Optional | Yes | No | Optional filter for the facility name, such as adult fitness or Toilet near West Entrance; blank for most rows (13,172 of 14,754); wildcards work. |
| `location[latitude]` | Optional | No | Yes | Optional filter for latitude in decimal degrees (WGS84); use a range to search a box. Values run from 1.2136 to 1.4632. |
| `location[longitude]` | Optional | No | Yes | Optional filter for longitude in decimal degrees (WGS84); use a range to search a box. Values run from 103.6897 to 104.0167. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

Bracket parameters such as `location[latitude]` need `curl -g` (or `%5B` and `%5D`) so that curl does not read the brackets as a glob.

## Response fields

Nested objects are shown with dots, for example `address.street` is the `street` key inside the `address` object.

| Field | Type | Description |
|---|---|---|
| `name` | string | The facility name, such as adult fitness or Toilet near West Entrance; blank for most rows (13,172 of 14,754). |
| `class` | string | The facility kind in capitals, such as CARPARK, TOILET, PLAYGROUND, BBQ PIT or FITNESS AREA (24 kinds). |
| `location.latitude` | number | Latitude in decimal degrees (WGS84). |
| `location.longitude` | number | Longitude in decimal degrees (WGS84). |

## Example

All toilets in parks.

```bash
curl -s "https://api.jael.ee/JLEE/sg_nparks_park_facilities?class=TOILET"
```

Response (132 rows, first 2 shown):

```json
[
  {
    "name": "Toilet",
    "class": "TOILET",
    "location": {
      "latitude": 1.305722,
      "longitude": 103.867292
    }
  },
  {
    "name": "Area 2 Toilet cum bicycle kiosk",
    "class": "TOILET",
    "location": {
      "latitude": 1.292067,
      "longitude": 103.766146
    }
  }
]
```

Another call:

Playgrounds with Hill in the name.

```bash
curl -s "https://api.jael.ee/JLEE/sg_nparks_park_facilities?class=PLAYGROUND&name=*Hill*"
```

Returns 1 row.

## Data source

Park facilities, National Parks Board, data.gov.sg (https://data.gov.sg), dataset d_14d807e20158338fd578c2913953516e, Singapore Open Data Licence version 1.0 (https://data.gov.sg/open-data-licence). Source last updated September 9, 2026.

[Back to all APIs](../../README.md)
