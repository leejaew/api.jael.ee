# Dubai Korean Restaurants

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 15](https://img.shields.io/badge/records-15-lightgrey)

Korean restaurants, fast food places and cafes in the emirate of Dubai that OpenStreetMap tags as Korean, 15 rows, with cuisine tags, street, area, phone, website, opening hours and coordinates. Coverage is small and only reflects what mappers have tagged; filter by name, cuisine, place_type or area, or use a latitude and longitude range.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/dxb_korean_restaurants` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: none. Optional: 13 fields. Disallowed: 1 field.

Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `__id` | Optional | No | Yes | Row ID. Range supported, for example `__id=3..10`. |
| `name` | Optional | Yes | No | Optional filter for the venue name, such as Kyochon or Hanok Korean Restaurant. |
| `cuisine` | Optional | Yes | No | Optional filter for the OSM cuisine tags, separated by semicolons; values are korean (8), barbecue; korean (1), korean; sushi (1), grill; korean (1); blank for 3 of 15 rows. |
| `place_type` | Optional | No | No | Optional exact filter for the kind of venue; values are restaurant (12), fast food (2), cafe (1). |
| `address[street]` | Optional | Yes | No | Optional filter for the street name from OSM address tags, such as Oud Metha Road or Sheikh Zayed Road; blank for 13 of 15 rows. |
| `address[area]` | Optional | Yes | No | Optional filter for the neighbourhood, derived by reverse geocoding the point, such as Oud Metha or Al Safa 1. |
| `address[city]` | Optional | Yes | No | Optional filter for the city; always Dubai here, such as Dubai. |
| `address[emirate]` | Optional | No | No | Optional exact filter for the emirate; always Dubai here, such as Dubai. |
| `phone` | Optional | No | No | Optional exact filter for the phone number as published, kept as text; some rows hold two numbers separated by a semicolon, such as +97143344294 or +971 4 491 9633 ; +971 58 548 0089; blank for 11 of 15 rows. |
| `opening_hours` | Optional | Yes | No | Optional filter for the OSM opening hours text, such as Mo-Su 11:00-15:00, 18:00-23:00 or Mo-Su 09:30-00:00; blank for 9 of 15 rows. |
| `location[latitude]` | Optional | No | Yes | Optional filter for latitude in decimal degrees (WGS84); use a range to search a box; values run from 25.076795 to 25.289827. |
| `location[longitude]` | Optional | No | Yes | Optional filter for longitude in decimal degrees (WGS84); use a range to search a box; values run from 55.146348 to 55.403511. |
| `osm_id` | Optional | No | No | Optional exact filter for the OSM object id, such as node/4500044689 or node/4919128715. |
| `website` | Disallowed | No | No | Venue website link; returned in results, not used as a filter, such as https://order.radyes.co/nene_chicken or https://koreandxb.com; blank for 12 of 15 rows. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

Bracket parameters such as `location[latitude]` need `curl -g` (or `%5B` and `%5D`) so that curl does not read the brackets as a glob.

## Response fields

Nested objects are shown with dots, for example `address.street` is the `street` key inside the `address` object.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Row ID. |
| `name` | string | The venue name, such as Kyochon or Hanok Korean Restaurant. |
| `cuisine` | string | The OSM cuisine tags, separated by semicolons; values are korean (8), barbecue; korean (1), korean; sushi (1), grill; korean (1); blank for 3 of 15 rows. |
| `place_type` | string | The kind of venue; values are restaurant (12), fast food (2), cafe (1). |
| `address.street` | string | The street name from OSM address tags, such as Oud Metha Road or Sheikh Zayed Road; blank for 13 of 15 rows. |
| `address.area` | string | The neighbourhood, derived by reverse geocoding the point, such as Oud Metha or Al Safa 1. |
| `address.city` | string | The city; always Dubai here, such as Dubai. |
| `address.emirate` | string | The emirate; always Dubai here, such as Dubai. |
| `phone` | string | The phone number as published, kept as text; some rows hold two numbers separated by a semicolon, such as +97143344294 or +971 4 491 9633 ; +971 58 548 0089; blank for 11 of 15 rows. |
| `website` | string | Venue website link; returned in results, not used as a filter, such as https://order.radyes.co/nene_chicken or https://koreandxb.com; blank for 12 of 15 rows. |
| `opening_hours` | string | The OSM opening hours text, such as Mo-Su 11:00-15:00, 18:00-23:00 or Mo-Su 09:30-00:00; blank for 9 of 15 rows. |
| `location.latitude` | number | Latitude in decimal degrees (WGS84). |
| `location.longitude` | number | Longitude in decimal degrees (WGS84). |
| `osm_id` | string | The OSM object id, such as node/4500044689 or node/4919128715. |

## Example

The two fast food outlets, both Nene Chicken branches.

```bash
curl -s "https://api.jael.ee/JLEE/dxb_korean_restaurants?place_type=fast%20food"
```

Response (2 rows):

```json
[
  {
    "__id": 4,
    "name": "Nene Chicken Korean Restaurant",
    "cuisine": "korean",
    "place_type": "fast food",
    "address": {
      "street": null,
      "area": "Port Saeed",
      "city": "Dubai",
      "emirate": "Dubai"
    },
    "phone": "+971 4 491 9633 ; +971 58 548 0089",
    "website": "https://order.radyes.co/nene_chicken",
    "opening_hours": "Mo-Su 09:30-00:00",
    "location": {
      "latitude": 25.250686,
      "longitude": 55.332931
    },
    "osm_id": "node/10010671032"
  },
  {
    "__id": 5,
    "name": "Nene Chicken Korean Restaurant",
    "cuisine": "korean",
    "place_type": "fast food",
    "address": {
      "street": null,
      "area": "Al Quoz Industrial 3",
      "city": "Dubai",
      "emirate": "Dubai"
    },
    "phone": "+971 4 321 0433 ; +971 58 562 6488",
    "website": "https://order.radyes.co/nene_chicken",
    "opening_hours": "Fr-Su 10:00-00:00; Mo-Th 10:00-23:00",
    "location": {
      "latitude": 25.115955,
      "longitude": 55.203995
    },
    "osm_id": "node/10010671033"
  }
]
```

Another call:

Korean venues that also serve sushi.

```bash
curl -sg "https://api.jael.ee/JLEE/dxb_korean_restaurants?cuisine=*sushi*"
```

Returns 1 row.

## Data source

Data © OpenStreetMap contributors, available under the [Open Database License](https://www.openstreetmap.org/copyright). Neighborhood names come from OpenStreetMap Nominatim reverse lookups, so they are approximate. Extracted 2026-10-27. Coverage is small (15 venues) and only reflects what mappers have tagged.

[Back to all APIs](../../README.md)
