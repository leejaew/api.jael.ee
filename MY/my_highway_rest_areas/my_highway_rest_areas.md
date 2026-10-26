# Malaysia Highway Rest Areas

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 82](https://img.shields.io/badge/records-82-lightgrey)

The 82 PLUS Malaysia stops (R&R rest areas, lay-bys, overhead bridge restaurants and rest and service areas) on PLUS expressways, one row per place and travel direction, with kilometer marker, facility yes or no flags, petrol and EV charging brands and coordinates. Filter by state, highway, direction, stop type or any facility, or use a latitude and longitude range.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/my_highway_rest_areas` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: 1 field. Optional: 19 fields. Disallowed: 2 fields.

Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `address[state]` | Required | Yes | No | Required. Allowed values: Perak (20); Johor (14); Negeri Sembilan (10); Selangor (10); Terengganu (10); Penang (7); Kedah (6); Melaka (5). Use * to list all rows. |
| `__id` | Optional | No | Yes | Row ID. Range supported, for example `__id=3..10`. |
| `name` | Optional | Yes | No | Optional filter for the place name, such as Tapah, Gurun or Sungai Buloh Lay-by; each place appears once per direction, so most names have 2 rows. |
| `stop_type` | Optional | No | No | Optional exact filter for the kind of stop: R&R (full rest and relax area; 24 rows), Lay-by (small stop; 42), OBR (overhead bridge restaurant; 6) or RSA (rest and service area on LPT2; 10). |
| `highway` | Optional | Yes | No | Optional filter for the expressway name, such as North South Expressway (58 rows), East Coast Expressway (LPT2) or Second Link Expressway; blank for 8 rows where the highway is not known. |
| `highway_ref` | Optional | No | No | Optional exact filter for the OpenStreetMap route code: E1 (34 rows), E2 (24), E8 (10), E3, E6 or E15; blank for 8 rows. |
| `direction` | Optional | No | No | Optional exact filter for the travel direction the stop serves: Northbound, Southbound, Eastbound or Westbound (only Penanti and Mambau use east and west). |
| `km` | Optional | No | Yes | Optional filter for the kilometer marker as printed by PLUS, a number from 4.9 to 458.3; markers run from Johor Bahru northward on the North South Expressway but restart on the other highways, so use it together with highway. |
| `customer_service_phone` | Optional | No | No | Optional exact filter for the PLUS customer service phone as text: +603 7666 4666 (72 rows) or +609 681 7755 (10 rows on LPT2). |
| `toilet` | Optional | No | No | Optional exact filter, yes or no: whether PLUS lists a toilet; 80 yes, 2 no. |
| `surau` | Optional | No | No | Optional exact filter, yes or no: whether PLUS lists a surau (prayer room); 52 yes, 30 no. |
| `nappy_room` | Optional | No | No | Optional exact filter, yes or no: whether PLUS lists a nappy room; 42 yes, 40 no. |
| `petrol_station` | Optional | No | No | Optional exact filter, yes or no: whether PLUS lists a petrol station; 52 yes, 30 no. |
| `foodcourt` | Optional | No | No | Optional exact filter, yes or no: whether PLUS lists a foodcourt; 52 yes, 30 no. |
| `ev_charging` | Optional | No | No | Optional exact filter, yes or no: whether PLUS lists an EV charging station; 30 yes, 52 no. |
| `petrol_brand` | Optional | Yes | No | Optional filter for the petrol brands listed at the stop, such as Shell, Petronas, Petron, BHP or Caltex; several brands are joined with a slash; blank for 30 rows. |
| `ev_brand` | Optional | Yes | No | Optional filter for the EV charging operator listed, such as TNB Electron, JomCharge, ChargEV, Gentari or Shell Recharge; blank for 52 rows. |
| `location[latitude]` | Optional | No | Yes | Optional filter for latitude in decimal degrees (WGS84); use a range to search a box. Values run from 1.6574 to 5.9662; blank for 16 rows with no safe map match. |
| `location[longitude]` | Optional | No | Yes | Optional filter for longitude in decimal degrees (WGS84); use a range to search a box. Values run from 100.4177 to 103.507; blank for 16 rows with no safe map match. |
| `osm_id` | Optional | No | No | Optional exact filter for the OpenStreetMap element used for the coordinates, such as w317665021 (w for way, n for node, r for relation); blank for 16 rows. |
| `operator` | Disallowed | Yes | No | Optional filter for the highway operator; every row is PLUS Malaysia Berhad. |
| `coordinate_match` | Disallowed | No | No | Optional exact filter for how the coordinates were matched: carriageway_side (40 rows), name_with_direction (11), name_shared_both_directions (8), name_single (7); blank for 16 rows. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

Bracket parameters such as `location[latitude]` need `curl -g` (or `%5B` and `%5D`) so that curl does not read the brackets as a glob.

## Response fields

Nested objects are shown with dots, for example `address.street` is the `street` key inside the `address` object.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Row ID. |
| `name` | string | The place name, such as Tapah, Gurun or Sungai Buloh Lay-by; each place appears once per direction, so most names have 2 rows. |
| `stop_type` | string | The kind of stop: R&R (full rest and relax area; 24 rows), Lay-by (small stop; 42), OBR (overhead bridge restaurant; 6) or RSA (rest and service area on LPT2; 10). |
| `operator` | string | The highway operator; every row is PLUS Malaysia Berhad. |
| `highway` | string | The expressway name, such as North South Expressway (58 rows), East Coast Expressway (LPT2) or Second Link Expressway; blank for 8 rows where the highway is not known. |
| `highway_ref` | string | The OpenStreetMap route code: E1 (34 rows), E2 (24), E8 (10), E3, E6 or E15; blank for 8 rows. |
| `direction` | string | The travel direction the stop serves: Northbound, Southbound, Eastbound or Westbound (only Penanti and Mambau use east and west). |
| `address.state` | string | Required. Allowed values: Perak (20); Johor (14); Negeri Sembilan (10); Selangor (10); Terengganu (10); Penang (7); Kedah (6); Melaka (5). Use * to list all rows. |
| `km` | number | The kilometer marker as printed by PLUS, a number from 4.9 to 458.3; markers run from Johor Bahru northward on the North South Expressway but restart on the other highways, so use it together with highway. |
| `customer_service_phone` | string | The PLUS customer service phone as text: +603 7666 4666 (72 rows) or +609 681 7755 (10 rows on LPT2). |
| `toilet` | string | Optional exact filter, yes or no: whether PLUS lists a toilet; 80 yes, 2 no. |
| `surau` | string | Optional exact filter, yes or no: whether PLUS lists a surau (prayer room); 52 yes, 30 no. |
| `nappy_room` | string | Optional exact filter, yes or no: whether PLUS lists a nappy room; 42 yes, 40 no. |
| `petrol_station` | string | Optional exact filter, yes or no: whether PLUS lists a petrol station; 52 yes, 30 no. |
| `foodcourt` | string | Optional exact filter, yes or no: whether PLUS lists a foodcourt; 52 yes, 30 no. |
| `ev_charging` | string | Optional exact filter, yes or no: whether PLUS lists an EV charging station; 30 yes, 52 no. |
| `petrol_brand` | string | The petrol brands listed at the stop, such as Shell, Petronas, Petron, BHP or Caltex; several brands are joined with a slash; blank for 30 rows. |
| `ev_brand` | string | The EV charging operator listed, such as TNB Electron, JomCharge, ChargEV, Gentari or Shell Recharge; blank for 52 rows. |
| `location.latitude` | string | Latitude in decimal degrees (WGS84). |
| `location.longitude` | string | Longitude in decimal degrees (WGS84). |
| `osm_id` | string | The OpenStreetMap element used for the coordinates, such as w317665021 (w for way, n for node, r for relation); blank for 16 rows. |
| `coordinate_match` | string | How the coordinates were matched: carriageway_side (40 rows), name_with_direction (11), name_shared_both_directions (8), name_single (7); blank for 16 rows. |

## Example

Perak stops with EV charging.

```bash
curl -sg "https://api.jael.ee/JLEE/my_highway_rest_areas?address[state]=Perak&ev_charging=yes"
```

Response (11 rows, first 2 shown):

```json
[
  {
    "__id": 46,
    "name": "Behrang",
    "stop_type": "Lay-by",
    "operator": "PLUS Malaysia Berhad",
    "highway": "North South Expressway",
    "highway_ref": "E1",
    "direction": "Southbound",
    "address": {
      "state": "Perak"
    },
    "km": 382.8,
    "customer_service_phone": "+603 7666 4666",
    "toilet": "yes",
    "surau": "no",
    "nappy_room": "yes",
    "petrol_station": "yes",
    "foodcourt": "yes",
    "ev_charging": "yes",
    "petrol_brand": "Petron",
    "ev_brand": "Gentari",
    "location": {
      "latitude": 3.782676,
      "longitude": 101.446384
    },
    "osm_id": "w857846131",
    "coordinate_match": "carriageway_side"
  },
  {
    "__id": 47,
    "name": "Bukit Gantang",
    "stop_type": "Lay-by",
    "operator": "PLUS Malaysia Berhad",
    "highway": "North South Expressway",
    "highway_ref": "E1",
    "direction": "Northbound",
    "address": {
      "state": "Perak"
    },
    "km": 225.4,
    "customer_service_phone": "+603 7666 4666",
    "toilet": "yes",
    "surau": "yes",
    "nappy_room": "no",
    "petrol_station": "yes",
    "foodcourt": "yes",
    "ev_charging": "yes",
    "petrol_brand": "Caltex",
    "ev_brand": "EVC",
    "location": {
      "latitude": 4.772746,
      "longitude": 100.762404
    },
    "osm_id": "w445962693",
    "coordinate_match": "carriageway_side"
  }
]
```

Another call:

Northbound R&R areas with a surau.

```bash
curl -sg "https://api.jael.ee/JLEE/my_highway_rest_areas?address[state]=*&stop_type=R%26R&direction=Northbound&surau=yes"
```

Returns 11 rows.

## Data source

Names, directions, kilometre marks and facilities from the public PLUS Malaysia facilities pages (facts only). Coordinates and highway names: Data © OpenStreetMap contributors, available under the [Open Database License](https://www.openstreetmap.org/copyright).

[Back to all APIs](../../README.md)
