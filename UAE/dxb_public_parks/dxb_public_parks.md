# Dubai Public Parks

![access public](https://img.shields.io/badge/access-public-brightgreen) ![format JSON](https://img.shields.io/badge/format-JSON-blue) ![method GET](https://img.shields.io/badge/method-GET-informational) ![records 183](https://img.shields.io/badge/records-183-lightgrey)

Public parks, gardens, dog parks and nature reserves in the emirate of Dubai, 183 rows from OpenStreetMap, with area, park type, entry fee, opening hours, operator, access, wheelchair access and coordinates. Official Dubai Pulse park data could not be reached, so fee and hours are filled only where mappers tagged them. Filter by name, park_type or area, or use a latitude and longitude range.

## Endpoint

| | |
|---|---|
| Base URL | `https://api.jael.ee/JLEE/` |
| API URL | `https://api.jael.ee/JLEE/dxb_public_parks` |
| Method | `GET` |
| Authentication | None. The API is public. |
| Response | JSON array, one object per row |

## Query parameters

Required: none. Optional: 10 fields. Disallowed: 6 fields.

Disallowed means the field is returned in the output but cannot be used as a filter.

| Parameter | Filter | Wildcard | Range | Description |
|---|---|---|---|---|
| `__id` | Optional | No | Yes | Row ID. Range supported, for example `__id=3..10`. |
| `name` | Optional | Yes | No | Optional filter for the park name, such as Safa Park or Creek Park. |
| `name_ar` | Optional | Yes | No | Optional filter for the Arabic name, such as حديقة البرشاء ٣ or حديقة الخزّان; blank for 144 of 183 rows. |
| `park_type` | Optional | No | No | Optional exact filter for the kind of green space; values are park (166), nature reserve (8), garden (6), dog park (3). |
| `address[area]` | Optional | Yes | No | Optional filter for the neighbourhood, derived by reverse geocoding the point, such as Jumeirah Village Circle or Hor Al Anz East; blank for 1 of 183 rows. |
| `address[city]` | Optional | Yes | No | Optional filter for the city; Dubai or Hatta; values are Dubai (180), Hatta (3). |
| `address[emirate]` | Optional | No | No | Optional exact filter for the emirate; Dubai for every row; values are Dubai (183). |
| `location[latitude]` | Optional | No | Yes | Optional filter for latitude in decimal degrees (WGS84); use a range to search a box; values run from 24.76958 to 25.321864. |
| `location[longitude]` | Optional | No | Yes | Optional filter for longitude in decimal degrees (WGS84); use a range to search a box; values run from 54.912146 to 56.132194. |
| `osm_id` | Optional | No | No | Optional exact filter for the OSM object id, such as way/360082076 or way/1274077937. |
| `fee_required` | Disallowed | No | No | Whether an entry fee applies (yes or no) from the OSM fee tag; values are yes (6), no (1); blank for 176 of 183 rows. |
| `entry_fee` | Disallowed | No | No | The fee text where tagged, such as 5 AED or 55 AED; blank for 178 of 183 rows. |
| `opening_hours` | Disallowed | No | No | The OSM opening hours text, such as 24/7 or 07:00-22:00; blank for 167 of 183 rows. |
| `operator` | Disallowed | No | No | The operator as tagged; mixed spelling and some Arabic; values are بلدية دبي (6), Cityland (2), Dubai Municipality (2), DUBAI MUNICIPALITY (1); blank for 168 of 183 rows. |
| `access` | Disallowed | No | No | The OSM access tag; values are yes (8), customers (1); blank for 174 of 183 rows. |
| `wheelchair` | Disallowed | No | No | The OSM wheelchair tag; values are yes (3), limited (2); blank for 178 of 183 rows. |

* Parameters are combined with AND. Text matches ignore case. See [How filters work](../../README.md#how-filters-work) for the wildcard and range syntax.

Bracket parameters such as `location[latitude]` need `curl -g` (or `%5B` and `%5D`) so that curl does not read the brackets as a glob.

## Response fields

Nested objects are shown with dots, for example `address.street` is the `street` key inside the `address` object.

| Field | Type | Description |
|---|---|---|
| `__id` | integer | Row ID. |
| `name` | string | The park name, such as Safa Park or Creek Park. |
| `name_ar` | string | The Arabic name, such as حديقة البرشاء ٣ or حديقة الخزّان; blank for 144 of 183 rows. |
| `park_type` | string | The kind of green space; values are park (166), nature reserve (8), garden (6), dog park (3). |
| `address.area` | string | The neighbourhood, derived by reverse geocoding the point, such as Jumeirah Village Circle or Hor Al Anz East; blank for 1 of 183 rows. |
| `address.city` | string | The city; Dubai or Hatta; values are Dubai (180), Hatta (3). |
| `address.emirate` | string | The emirate; Dubai for every row; values are Dubai (183). |
| `fee_required` | string | Whether an entry fee applies (yes or no) from the OSM fee tag; values are yes (6), no (1); blank for 176 of 183 rows. |
| `entry_fee` | string | The fee text where tagged, such as 5 AED or 55 AED; blank for 178 of 183 rows. |
| `opening_hours` | string | The OSM opening hours text, such as 24/7 or 07:00-22:00; blank for 167 of 183 rows. |
| `operator` | string | The operator as tagged; mixed spelling and some Arabic; values are بلدية دبي (6), Cityland (2), Dubai Municipality (2), DUBAI MUNICIPALITY (1); blank for 168 of 183 rows. |
| `access` | string | The OSM access tag; values are yes (8), customers (1); blank for 174 of 183 rows. |
| `wheelchair` | string | The OSM wheelchair tag; values are yes (3), limited (2); blank for 178 of 183 rows. |
| `location.latitude` | number | Latitude in decimal degrees (WGS84). |
| `location.longitude` | number | Longitude in decimal degrees (WGS84). |
| `osm_id` | string | The OSM object id, such as way/360082076 or way/1274077937. |

## Example

The three dog parks.

```bash
curl -s "https://api.jael.ee/JLEE/dxb_public_parks?park_type=dog%20park"
```

Response (3 rows, first 2 shown):

```json
[
  {
    "__id": 62,
    "name": "BBET Dog Park",
    "name_ar": null,
    "park_type": "dog park",
    "address": {
      "area": "Business Bay",
      "city": "Dubai",
      "emirate": "Dubai"
    },
    "fee_required": null,
    "entry_fee": null,
    "opening_hours": null,
    "operator": null,
    "access": null,
    "wheelchair": "yes",
    "location": {
      "latitude": 25.188655,
      "longitude": 55.264964
    },
    "osm_id": "node/10679928342"
  },
  {
    "__id": 81,
    "name": "Downtown Dog Park",
    "name_ar": null,
    "park_type": "dog park",
    "address": {
      "area": "Burj Khalifa",
      "city": "Dubai",
      "emirate": "Dubai"
    },
    "fee_required": null,
    "entry_fee": null,
    "opening_hours": null,
    "operator": null,
    "access": null,
    "wheelchair": null,
    "location": {
      "latitude": 25.186124,
      "longitude": 55.27691
    },
    "osm_id": "way/1528337950"
  }
]
```

Another call:

Parks in Business Bay.

```bash
curl -sg "https://api.jael.ee/JLEE/dxb_public_parks?address[area]=*Business%20Bay*"
```

Returns 14 rows.

## Data source

Data © OpenStreetMap contributors, available under the Open Database License (https://www.openstreetmap.org/copyright). Neighborhood names come from Nominatim reverse lookups and are approximate. Fee and hours fields are sparse. Dubai Pulse could not be reached when the data was built, so it is not used.

[Back to all APIs](../../README.md)
